+++
title = "Build a Key-Value store in C++"
date = "2025-12-18"

[taxonomies]
tags=["C++", "Databases", "Key-Value", "Redis"]

[extra]
repo_view = true
comment = true
+++

## Implementation of Key-Value Store

A key-value store is a type of database that uses a simple data model where each item is stored as a pair.
A key and its associated value where :

- Key: The unique identifier used to access the stored data.
- Value: The data associated with the key. It can be anything—text, numbers, objects, etc.

Usually a KV offers a vast avarienty of features including database management, user authentication and API to connect, but to build a simple KV store 
a very little effort is required. 
A simple KV store can be build in C++ using **std::unordered_map** which is a container that stores key-value pairs, but the key and the value types are fixed at compile time when you declare the map. This means that both the key and value types must be specified when you declare the unordered_map and they cannot change for any element in that map.

```cpp
// hashmap with Key of type int and value of type int
std::unordered_map<int, int> myMap;
```
<!-- 
To make a **std::unordered_map** store multiple datatypes we can declare our own generecic types of key and value, and we also have a define custom hashing function for our new types which can be done like this. -->

To create a std::unordered_map that can store multiple data types, we can define custom key and value types, and provide a custom hash function for those types. Here's how we can do it:

```cpp
using Key = std::variant<int, std::string>;  // Define the Key type as a variant of int and string
using Value = std::variant<int, std::string>; // Define the Value type as a variant of int and string

// Custom hash function for the Key type
struct KeyHash {
    size_t operator()(const Key& k) const {
        return std::visit([](auto&& v) {
            // For each variant value, compute the hash based on its type
            return std::hash<std::decay_t<decltype(v)>>{}(v);  
        }, k);
    }
};

// Declare an unordered_map with the custom key, value, and hash function
std::unordered_map<Key, Value, KeyHash> in_mem;
```


#### Definitions of Functions/Methods Used:

- ` std::variant<int, std::string> `:
    - std::variant is a type-safe union in C++ that can hold one of the specified types at any given time. In this case, the Key and Value types can hold either an int or a std::string.

- ` struct KeyHash `:
    - This is a custom struct that implements the hash function for the Key type. In std::unordered_map, a hash function is required to compute a hash value for the key, which is used for efficient lookups.

    - The hash function for the Key type is implemented in the operator() method of the KeyHash struct.

- ` operator()(const Key& k) const `:
    - This is the function call operator for the KeyHash struct. It takes a Key as an argument and returns a hash value of type size_t.

    - The std::visit function is used here to handle the std::variant, allowing the hash function to work on each possible type inside the variant (i.e., int or std::string).

- ` std::visit([](auto&& v) { ... }, k) `:
    - std::visit is a C++17 feature that applies a visitor function (the lambda [](auto&& v) { ... }) to the active element in the std::variant object. It decodes the type stored in the std::variant and computes the hash accordingly.

    - This allows the hash function to be generic and applicable to any type stored in the variant (in this case, int or std::string).

- ` std::hash<std::decay_t<decltype(v)>>{}(v) `:

    - This part of the code computes the hash of the value v. It uses std::hash to create a hash for the type v, which can be either int or std::string in this case.

    - std::decay_t<decltype(v)> is used to remove any reference and const-qualifiers from the type of v, ensuring that std::hash works correctly for both int and std::string.

- ` std::unordered_map<Key, Value, KeyHash> `:

    - This declares an unordered map where the key is of type Key (which is a std::variant<int, std::string>) and the value is of type Value (which is also a std::variant<int, std::string>). The custom hash function KeyHash is used to hash the keys of the map.


```cpp
#include <iostream>
#include <unordered_map>
#include <string>
#include <variant>
#include <functional>
#include <optional>

using Key = std::variant<int, std::string>;
using Value = std::variant<int, std::string>;

struct KeyHash{
  size_t operator()(const Key& k) const {
    return std::visit([](auto&& v) {
        return std::hash<std::decay_t<decltype(v)>>{}(v);
    }, k);
  }
};

std::unordered_map<Key, Value, KeyHash> in_mem;


template <typename K, typename V>
void Insert(K&& key, V&& value){
  in_mem.emplace(
    Key{std::forward<K>(key)},
    Value{std::forward<V>(value)}
  );
}

auto Get(const Key& key){
  auto it = in_mem.find(key);
  if( it == in_mem.end()){
    std::cout<<"Item not found"<<std::endl;
    return;
  }
  std::visit([](auto&& v){
      std::cout<<v<<std::endl;
  }, it->second);
  // std::cout<<result<<endl;
}

int main(){

  std::string mode = "post";

  while(true){

    std::string input;

    std::cout<< mode << ">";
    std::getline(std::cin, input);

    if(input == "exit") break;

    if(input == "get") { mode = "get"; continue; }

    if(input == "post") { mode = "post"; continue; }

    if(mode == "post"){
      auto pos = input.find(":");
      if(pos == std::string::npos) std::cout<< "Invalid format!";

      std::string key_str = input.substr(0, pos);
      std::string value_str = input.substr(pos + 1);

      Insert(Key{key_str}, Value{value_str});
    }

    if(mode == "get"){
      std::string key_str = input;

      Get(Key{key_str});
    }

  }
  return 0;
 }
```