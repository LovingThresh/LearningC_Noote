# CodeExample

```c++
#include <iostream>
#include <string>
#include <vector>
#include <iterator>
#include <memory>
#include <format>
#include <sstream>
#include <algorithm>
#include <ranges>
#include <numeric>
#include <chrono>

int main() {
    // 使用传统STL处理Vector，选择所有偶数，偶数翻倍，选择前1000个元素
    std::vector<int> data(10000);
    std::iota(data.begin(), data.end(), 1); // Fill with 1 to 10000

    const auto start = std::chrono::high_resolution_clock::now();

    // Step 1: Filter even numbers
    std::vector<int> evens;
    std::ranges::copy_if(data.begin(), data.end(), std::back_inserter(evens), [](const int x) { return x % 2 == 0; });

    // Step 2: Double the values
    std::vector<int> doubled;
    std::ranges::transform(evens.begin(), evens.end(), std::back_inserter(doubled), [](const int x) { return x * 2; });

    // Step 3: Select the first 1000 elements
    std::vector<int> first1000(doubled.begin(), doubled.begin() + 1000);

    const auto end = std::chrono::high_resolution_clock::now();
    const std::chrono::duration<double> elapsed = end - start;
    std::cout << "Traditional STL processing time: " << elapsed.count() * 1000 << " milli seconds." << std::endl;

    // 使用传统STL优化处理Vector的，选择前2000个元素选择所有偶数，偶数翻倍，
    std::vector<int> data_optimized(10000);
    std::iota(data_optimized.begin(), data_optimized.end(), 1);  // Fill with 1 to 10000

    const auto start_optimized = std::chrono::high_resolution_clock::now();

    // Step 1: Take the first 2000 elements
    std::vector<int> first2000_optimized(data_optimized.begin(), data_optimized.begin() + 2000);

    // Step 2: Filter even numbers
    std::vector<int> evens_optimized;
    std::ranges::copy_if(first2000_optimized.begin(), first2000_optimized.end(), std::back_inserter(evens_optimized), [](const int x) { return x % 2 == 0; });

    // Step 3: Double the values
    std::vector<int> doubled_optimized;
    std::ranges::transform(evens_optimized.begin(), evens_optimized.end(), std::back_inserter(doubled_optimized), [](const int x) { return x * 2; });

    // Step 4: Select the first 1000 elements, if there are at least 1000
    std::vector<int> first1000_optimized;
    const auto count_to_take = std::min(static_cast<size_t>(1000), doubled_optimized.size());
    std::copy_n(doubled_optimized.begin(), count_to_take, std::back_inserter(first1000_optimized));

    const auto end_optimized = std::chrono::high_resolution_clock::now();
    const std::chrono::duration<double> elapsed_optimized = end_optimized - start_optimized;
    std::cout << "Optimized STL processing time: " << elapsed_optimized.count() * 1000 << " milli seconds." << std::endl;

    // 使用Ranges处理Vector
    std::vector<int> data_ranges(10000);
    std::iota(data_ranges.begin(), data_ranges.end(), 1);  // Fill with 1 to 10000

    const auto start_ranges = std::chrono::high_resolution_clock::now();

    auto result_ranges = data_ranges
        | std::views::take(2000)  // Take only the first 2000 elemens
        | std::views::filter([](const int x) { return x % 2 == 0; })  // Filter even numbers
        | std::views::transform([](const int x) { return x * 2; })  // Double the value
        | std::ranges::to<std::vector>();

    // Convert to vector (only now the computations happen)

    const auto end_ranges = std::chrono::high_resolution_clock::now();
    const std::chrono::duration<double> elapsed_ranges = end_ranges - start_ranges;
    std::cout << "Ranges processing time: " << elapsed_ranges.count() * 1000 << " milli seconds." << std::endl;
    
    // clang++ -stdlib=libstdc++ -O3 -DNDEBUG -std=gnu++23
    // Traditional STL processing time: 0.07896 milli seconds.
	// Optimized STL processing time: 0.009016 milli seconds.
	// Optimized Ranges processing time: 0.004647 milli seconds.
	// Python Ranges processing time: 0.18906593322753906 milli seconds
    return 0;

}

```