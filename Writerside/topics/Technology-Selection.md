# Technology Selection

### 项目背景
在当前的项目中，需要对表格数据进行高效的管理和展示。表格中的数据分为行（`Markup`）和列（`Column`），其中列包含默认列和自定义列。项目的需求包括高效的数据查找、插入、删除操作，以及与 Qt 的 MVD（Model-View-Delegate）架构结合，保持界面交互的高性能。

为了选择最佳的数据结构来管理这些表格数据，我们需要在不同的实现方式中权衡性能、代码复杂度和可扩展性。

---

### 需求分析
1. **数据查找性能**：
	- 行数据（`Markup`）的查找应该是高效的，适合频繁访问。
	- 列数据（`Column`）既需要通过列名快速查找，也需要根据列的可见性进行动态处理。

2. **可维护性与灵活性**：
	- 代码需要易于扩展，尤其是列的添加、删除、隐藏、显示等操作。
	- 隐藏列和显示列的管理应保持简单，不引入过多的复杂性。

3. **与 Qt MVD 的集成**：
	- 数据结构需要与 `QAbstractItemModel` 进行有效结合，保证在大数据量情况下的性能表现，避免性能瓶颈。

---

### 技术选型方案

#### 方案 1：基于 `std::vector` 和 `std::unordered_map`
1. **行数据**：使用 `std::vector<MarkupPtr>` 来存储行数据，每行是一个 `Markup` 对象指针。`std::vector` 提供了高效的顺序和随机访问，时间复杂度为 O(1)。

2. **列数据**：使用 `std::unordered_map<QString, Column>` 来存储列数据，支持通过列名快速查找。`std::unordered_map` 采用哈希表实现，查找、插入、删除的时间复杂度为 O(1)。

3. **实现细节**：
	- 需要自行管理列的显示和隐藏逻辑，可以通过额外的数据结构管理列的可见性。
	- 每次列的添加、删除和查找，均通过 `std::unordered_map` 进行，保持高效。

4. **优点**：
	- 高效的列查找：哈希表查找时间复杂度为 O(1)，特别适合大量列的情况下快速查找。
	- 行数据存储在 `std::vector` 中，访问行时性能优越，遍历效率较高。

5. **缺点**：
	- 代码复杂度较高，需要开发人员手动管理行和列的数据结构。
	- 对于列的显示和隐藏，需要额外的数据结构处理，增加了实现复杂度。

6. **适用场景**：
	- 大量列数据需要频繁查找和操作的场景。
	- 追求极致性能，特别是在大规模表格数据的管理和展示中。

---

#### 方案 2：基于 `QVector` 和列管理器（`ColumnManager`）
1. **行数据**：使用 `QVector<MarkupPtr>` 来存储行数据，与 `std::vector` 类似，`QVector` 也是动态数组，提供高效的顺序和随机访问，时间复杂度为 O(1)。

2. **列数据**：将列分为默认列（`m_oDefaultColumns`）和自定义列（`m_oCustomColumns`），分别存储在两个 `QVector<Column>` 中。通过 `ColumnManager` 进行列的管理，列的显示和隐藏通过 `m_oVisibleColumnIndices` 映射实现。

3. **实现细节**：
	- 列的查找通过 `m_oVisibleColumnIndices` 进行索引映射后访问 `QVector`，保证了列数据访问的效率。
	- 默认列和自定义列分开存储，使得列的添加和删除操作更为简单和清晰。

4. **优点**：
	- 简单的列管理逻辑：通过 `m_oVisibleColumnIndices` 映射列索引，能高效处理列的显示和隐藏，无需手动管理复杂的列数据结构。
	- 列和行数据都存储在 `QVector` 中，提供了较好的性能，特别是在行列的遍历上。
	- 代码简洁、易于维护，列的添加、删除和显示切换较为灵活。

5. **缺点**：
	- 列的查找需要通过映射进行索引转换，虽然是 O(1) 复杂度，但有额外的开销。
	- 在极端大规模列的场景下，性能可能略逊于哈希表的查找。

6. **适用场景**：
	- 适合中小规模的表格管理，尤其在列需要频繁显示和隐藏时，可以灵活处理列的可见性。
	- 代码维护性要求高的项目，列管理逻辑封装简洁，不易出错。

---

### 方案比较

| 特性            | 方案 1 (`std::vector` + `std::unordered_map`) | 方案 2 (`QVector` + `ColumnManager`) |
|---------------|---------------------------------------------|------------------------------------|
| **行数据的遍历**    | O(1)，高效遍历和随机访问                              | O(1)，高效遍历和随机访问                     |
| **列数据的查找**    | O(1)，哈希表查找                                  | O(1)，索引映射 + QVector 访问             |
| **列数据的插入/删除** | O(1)，哈希表插入和删除                               | O(1)，QVector 插入和删除                 |
| **代码复杂度**     | 高，手动管理行和列的数据结构                              | 中，列管理逻辑由 `ColumnManager` 负责        |
| **列的显示/隐藏**   | 需要额外管理                                      | 通过索引映射，逻辑简单                        |
| **维护成本**      | 高，需要手动管理复杂的数据结构                             | 低，代码结构清晰，易于扩展                      |
| **适用场景**      | 适合大规模表格和频繁列查找操作                             | 适合中小规模表格，列显示和隐藏频繁                  |

---

### 方案推荐

基于对性能、代码复杂度、可扩展性和维护成本的全面分析，推荐使用 **方案 2**（基于 `QVector` 和 `ColumnManager`）：

- **性能足够优秀**：在中小规模表格管理中，列查找和显示/隐藏的性能表现优秀，能够很好地应对日常需求。
- **代码维护性高**：`ColumnManager` 通过索引映射管理列的显示与隐藏，代码结构清晰且维护成本低，适合长期开发和维护。
- **灵活性强**：分离默认列和自定义列，并通过 `m_oVisibleColumnIndices` 控制列的显示顺序，适合频繁调整列的 UI 场景。

对于列数非常多且需要极高查找效率的场景，可以考虑 **方案 1**，但在大部分情况下，**方案 2** 更为合适。

---

### 结论
在 Qt 项目的表格数据管理中，选择合适的数据结构至关重要。`QVector` 与 `ColumnManager` 结合的方式在平衡性能和代码简洁性上表现较好，适合大多数项目的实际需求。通过高效的索引映射和简单的列管理逻辑，该方案可以在不牺牲性能的前提下，提供良好的可扩展性和代码维护性。
