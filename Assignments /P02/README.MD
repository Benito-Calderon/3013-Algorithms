#Benito Calderon  
##3013-Algorithms



#include <iostream>
#include <fstream>
#include <string>
#include <chrono>
#include <vector>
#include "json.hpp"
#include "termcolor.hpp"

using json = nlohmann::json;
using namespace std::chrono;

// Function to load JSON data from file
json loadJson(const std::string& filename) 
{
    std::ifstream file(filename);
    if (!file.is_open()) 
    {
        throw std::runtime_error("Error opening file: " + filename);
    }

    json data;
    file >> data;
    file.close();

    return data;
}

// Function to perform autosuggestions
void autoSuggest(const std::string& prefix, const std::vector<std::string>& dictionary)
{
    auto start = high_resolution_clock::now();

    std::vector<std::string> suggestions;
    for (const std::string& word : dictionary)
      {
        if (word.find(prefix) == 0)
        {
            suggestions.push_back(word);
        }
        if (suggestions.size() >= 10) 
        {
            break;
        }
    }

    auto stop = high_resolution_clock::now();
    auto duration = duration_cast<microseconds>(stop - start);

    std::cout << termcolor::bold << termcolor::yellow << prefix << std::endl;
    std::cout << termcolor::reset;
    std::cout << suggestions.size() << " words found in " << duration.count() / 1000000.0 << " seconds\n";

    for (const std::string& suggestion : suggestions) 
    {
        std::cout << suggestion << " ";
    }
    std::cout << std::endl;
}

int main()
{
    // Load JSON data
    auto start_load = high_resolution_clock::now();
    json dictionary = loadJson("dictionary.json");
    auto stop_load = high_resolution_clock::now();
    auto duration_load = duration_cast<microseconds>(stop_load - start_load);
    std::cout << "Data loaded in " << duration_load.count() / 1000000.0 << " seconds\n";

    // Extract words from JSON data
    std::vector<std::string> words;
    for (const auto& entry : dictionary.items())
    {
        words.push_back(entry.key());
    }

    // User input for autosuggestions
    std::string input;
    std::cout << "Type a word to get autosuggestions: ";
    std::cin >> input;

    // Perform autosuggestions
    autoSuggest(input, words);

    return 0;
}
