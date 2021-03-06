---
title: "uniform_int_distribution Class | Microsoft Docs"
ms.custom: ""
ms.date: "11/04/2016"
ms.reviewer: ""
ms.suite: ""
ms.technology: 
  - "devlang-cpp"
ms.tgt_pltfrm: ""
ms.topic: "article"
f1_keywords: 
  - "uniform_int_distribution"
  - "std::uniform_int_distribution"
  - "random/std::uniform_int_distribution"
  - "std::uniform_int_distribution::reset"
  - "random/std::uniform_int_distribution::reset"
  - "std::uniform_int_distribution::a"
  - "random/std::uniform_int_distribution::a"
  - "std::uniform_int_distribution::b"
  - "random/std::uniform_int_distribution::b"
  - "std::uniform_int_distribution::param"
  - "random/std::uniform_int_distribution::param"
  - "std::uniform_int_distribution::min"
  - "random/std::uniform_int_distribution::min"
  - "std::uniform_int_distribution::max"
  - "random/std::uniform_int_distribution::max"
  - "std::uniform_int_distribution::operator()"
  - "random/std::uniform_int_distribution::operator()"
  - "std::uniform_int_distribution::param_type"
  - "random/std::uniform_int_distribution::param_type"
  - "std::uniform_int_distribution::param_type::a"
  - "random/std::uniform_int_distribution::param_type::a"
  - "std::uniform_int_distribution::param_type::b"
  - "random/std::uniform_int_distribution::param_type::b"
  - "std::uniform_int_distribution::param_type::operator=="
  - "random/std::uniform_int_distribution::param_type::operator=="
  - "std::uniform_int_distribution::param_type::operator!="
  - "random/std::uniform_int_distribution::param_type::operator!="
dev_langs: 
  - "C++"
helpviewer_keywords: 
  - "uniform_int_distribution class"
ms.assetid: a1867dcd-3bd9-4787-afe3-4b62692c1d04
caps.latest.revision: 20
author: "corob-msft"
ms.author: "corob"
manager: "ghogen"
translation.priority.ht: 
  - "cs-cz"
  - "de-de"
  - "es-es"
  - "fr-fr"
  - "it-it"
  - "ja-jp"
  - "ko-kr"
  - "pl-pl"
  - "pt-br"
  - "ru-ru"
  - "tr-tr"
  - "zh-cn"
  - "zh-tw"
---
# uniform_int_distribution Class
Generates a uniform (every value is equally probable) integer distribution within an output range that is inclusive-inclusive.  
  
## Syntax  
```  
template<class IntType = int>
   class uniform_int_distribution {
public:    
   // types 
   typedef IntType result_type;    
   struct param_type;    
   
   // constructors and reset functions 
   explicit uniform_int_distribution(
      result_type a = 0, result_type b = numeric_limits<result_type>::max());
   explicit uniform_int_distribution(const param_type& parm);
   void reset();

   // generating functions 
   template <class URNG>  
      result_type operator()(URNG& gen);
   template <class URNG>  
      result_type operator()(URNG& gen, const param_type& parm);

   // property functions 
   result_type a() const;
   result_type b() const;
   param_type param() const;
   void param(const param_type& parm);
   result_type min() const;
   result_type max() const;
};  
```  
### Parameters  
*IntType*  
The integer result type, defaults to `int`. For possible types, see [\<random>](../standard-library/random.md).  
  
## Remarks  
The template class describes an inclusive-inclusive distribution that produces values of a user-specified integral type with a distribution so that every value is equally probable. The following table links to articles about individual members.  
  
||||  
|-|-|-|  
|[uniform_int_distribution::uniform_int_distribution](#uniform_int_distribution__uniform_int_distribution)|`uniform_int_distribution::a`|`uniform_int_distribution::param`|  
|`uniform_int_distribution::operator()`|`uniform_int_distribution::b`|[uniform_int_distribution::param_type](#uniform_int_distribution__param_type)|  
  
The property member `a()` returns the currently stored minimum bound of the distribution, while `b()` returns the currently stored maximum bound. For this distribution class, these minimum and maximum values are the same as those returned by the common property functions `min()` and `max()`.  
  
The property member `param()` sets or returns the `param_type` stored distribution parameter package.  

The `min()` and `max()` member functions return the smallest possible result and largest possible result, respectively.  
  
The `reset()` member function discards any cached values, so that the result of the next call to `operator()` does not depend on any values obtained from the engine before the call.  
  
The `operator()` member functions return the next generated value based on the URNG engine, either from the current parameter package, or the specified parameter package.
  
For more information about distribution classes and their members, see [\<random>](../standard-library/random.md).  
  
## Example  
  
```cpp  
// compile with: /EHsc /W4  
#include <random>   
#include <iostream>  
#include <iomanip>  
#include <string>  
#include <map>  
  
void test(const int a, const int b, const int s) {  
  
    // uncomment to use a non-deterministic seed  
    //    std::random_device rd;  
    //    std::mt19937 gen(rd());  
    std::mt19937 gen(1729);  
  
    std::uniform_int_distribution<> distr(a, b);  
  
    std::cout << "lower bound == " << distr.a() << std::endl;  
    std::cout << "upper bound == " << distr.b() << std::endl;  
  
    // generate the distribution as a histogram  
    std::map<int, int> histogram;  
    for (int i = 0; i < s; ++i) {  
        ++histogram[distr(gen)];  
    }  
  
    // print results  
    std::cout << "Distribution for " << s << " samples:" << std::endl;  
    for (const auto& elem : histogram) {  
        std::cout << std::setw(5) << elem.first << ' ' << std::string(elem.second, ':') << std::endl;  
    }  
    std::cout << std::endl;  
}  
  
int main()  
{  
    int a_dist = 1;  
    int b_dist = 10;  
  
    int samples = 100;  
  
    std::cout << "Use CTRL-Z to bypass data entry and run using default values." << std::endl;  
    std::cout << "Enter an integer value for the lower bound of the distribution: ";  
    std::cin >> a_dist;  
    std::cout << "Enter an integer value for the upper bound of the distribution: ";  
    std::cin >> b_dist;  
    std::cout << "Enter an integer value for the sample count: ";  
    std::cin >> samples;  
  
    test(a_dist, b_dist, samples);  
}  
```  
  
```Output  
Use CTRL-Z to bypass data entry and run using default values.
Enter an integer value for the lower bound of the distribution: 0
Enter an integer value for the upper bound of the distribution: 12
Enter an integer value for the sample count: 200
lower bound == 0
upper bound == 12
Distribution for 200 samples:
    0 :::::::::::::::
    1 :::::::::::::::::::::
    2 ::::::::::::::::::
    3 :::::::::::::::
    4 :::::::
    5 :::::::::::::::::::::
    6 :::::::::::::
    7 ::::::::::
    8 :::::::::::::::
    9 :::::::::::::
   10 ::::::::::::::::::::::
   11 :::::::::::::
   12 :::::::::::::::::
```  
  
## Requirements  
 **Header:** \<random>  
  
 **Namespace:** std  
  
##  <a name="uniform_int_distribution__uniform_int_distribution"></a>  uniform_int_distribution::uniform_int_distribution  
Constructs the distribution.  
  
```  
explicit uniform_int_distribution(
   result_type a = 0, result_type b = std::numeric_limits<result_type>::max());
explicit uniform_int_distribution(const param_type& parm);
```  
  
### Parameters  
*a*  
The lower bound for random values, inclusive.  
  
*b*  
The upper bound for random values, inclusive.  
  
*parm*  
The `param_type` structure used to construct the distribution.  
  
### Remarks  
**Precondition:** `a ≤ b`  
  
The first constructor constructs an object whose stored `a` value holds the value *a* and whose stored `b` value holds the value *b*.  
  
The second constructor constructs an object whose stored parameters are initialized from *parm*. You can obtain and set the current parameters of an existing distribution by calling the `param()` member function.  
  
##  <a name="uniform_int_distribution__param_type"></a>  uniform_int_distribution::param_type  
 Stores the parameters of the distribution.  
```cpp
struct param_type {  
   typedef uniform_int_distribution<result_type> distribution_type;  
   param_type(
      result_type a = 0, result_type b = std::numeric_limits<result_type>::max());
   result_type a() const;
   result_type b() const;

   bool operator==(const param_type& right) const;
   bool operator!=(const param_type& right) const;
   };  
```  

### Parameters  
*a*  
The lower bound for random values, inclusive.  
  
*b*  
The upper bound for random values, inclusive.  
  
*right*  
The `param_type` object to compare to this.  
  
### Remarks  
**Precondition:** `a ≤ b`  
  
This structure can be passed to the distribution's class constructor at instantiation, to the `param()` member function to set the stored parameters of an existing distribution, and to `operator()` to be used in place of the stored parameters.  
  
## See Also  
 [\<random>](../standard-library/random.md)



