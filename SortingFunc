#include <unordered_map>
#include <functional>
#include <iostream>
#include <vector>
#include <deque>

// I is any item whose list (vector) you want sorted to match the order of key
// H is custom hasher, you can make this as specific to your use case as you want to increase efficiency
template <typename I, class H = std::hash<I>>
struct Sorter{
    Sorter(const std::vector<I>& key){
        for(I i : key) {
            _Map[i] = 0;
        }
        _Key = key;
    }
    void Count(std::vector<I> array){
        for(I i : array) {
            _Map[i]++;
        }
    }
    std::vector<I> ReconstructedList(){
        std::vector<I> out;
        std::unordered_map<I, int, H> cashedMap = _Map;
        for(I i : _Key){
            while(cashedMap[i] > 0){
                out.push_back(i);
                cashedMap[i]--;
            }
        }
        return out;
    }
    std::unordered_map<I, int, H> GetMap(){
        return _Map;
    }
    std::vector<I> _Key;
    std::unordered_map<I, int, H> _Map;
};

namespace example
{ // For those interested about the custom hasher function
// Example struct
struct Point{
    Point(int x1, int y1) : x(x1), y(y1) {}
    int x;
    int y;
    bool operator == (const Point& p) {
		return x == p.x && y == p.y;
	}
};

// What it means for two Point objects to be equal
bool operator == (const Point& l, const Point& r){
    return l.x == r.x && l.y == r.y;
}

// Example hasher
struct PointHasher{
    // Overload () operator
    std::size_t operator () (const Point& p){
        // Hash components of struct
        std::size_t x = std::hash<int>{}(p.x);
        std::size_t y = std::hash<int>{}(p.y);
        // Mix component hashes
        return x ^ (y << 1);
    }
};

// Example call
Sorter<Point, PointHasher> Name ({Point(1,2), Point(3,4)}); // NOTE* THIS FUNCTION CALL DOES NOT WORK AT THE MOMENT - WILL BUG FIX LATER MAYBE IF I HAVE TIME
// etc, etc
}

// Utility
namespace utl
{
    std::vector<char> stvc(std::string in) {
		std::vector<char> out;
		for (char c : in) out.push_back(c);
		return out;
	}
}

int main()
{
    std::vector<char> key = utl::stvc("KEY_abcdefghijk");
    std::vector<char> inputList;
    int k = 500;
    for(int i = 0; i < k; i++){
        char cashe = key[std::hash<int>{}(i) % key.size()];
        inputList.push_back(cashe);
        std::cout << cashe << ", ";
    }

    Sorter<char> s (key);
    s.Count(inputList);
    
    std::cout << "\n\nCounts:\n";
    for(char c : key) std::cout << c << ": " << s.GetMap()[c] << ", ";
    std::cout << "\n\n";

    for(char c : s.ReconstructedList()) std::cout << c << ", ";
}
