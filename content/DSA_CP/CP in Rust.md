[[DSA_CP/index|DSA_CP/index]]

## Resources:
https://rustp.org/basic-programs/input-single-number/

https://codeforces.com/blog/entry/111573

## My template: 
```rust
use std::io;
use std::collections::*;
use std::i32::{MIN, MAX};

pub fn take_usize() -> usize {
    let mut input = String::new();
    io::stdin().read_line(&mut input).unwrap();

    return input.trim().parse().unwrap();
}

pub fn take_int() -> i32 {
    let mut input = String::new();
    io::stdin().read_line(&mut input).unwrap();

    return input.trim().parse().unwrap();
}

pub fn take_vector_usize() -> Vec<usize> {
    let mut input = String::new();
    io::stdin().read_line(&mut input).unwrap();
    
    let arr:Vec<usize> = input
        .trim()
        .split_whitespace()
        .map(|x| x.parse().unwrap())
        .collect();

    return arr;
}

pub fn take_vector_int() -> Vec<i32> {
    let mut input = String::new();
    io::stdin().read_line(&mut input).unwrap();
    
    let arr:Vec<i32> = input
        .trim()
        .split_whitespace()
        .map(|x| x.parse().unwrap())
        .collect();

    return arr;
}

pub fn take_string_as_vec() -> Vec<char>{
    let mut input = String::new();
    io::stdin().read_line(&mut input).unwrap();

    let arr:Vec<char> = input
        .trim()
        .chars()
        .collect();

    return arr;
}

pub fn vec_to_string(input:Vec<char>) -> String{
    return input.iter().collect::<String>();
}

pub fn cnt_digits(input:usize) -> usize{
    return ((input as f32).log10() as usize)+1;
}

pub fn reverse_num(mut input:usize) -> usize{
    let mut rev = 0;
    while input>0 {
        rev = rev*10 + input%10;
        input /= 10;
    }
    rev
}

pub fn gcd(mut a:usize, mut b:usize) -> usize{
    // Euclidean Algorithm
    while a>0 && b>0 {
        if a>b {
            a=a%b;
        }
        else {
            b=b%a;
        }
    }
    if a==0 {
        return b;
    }
    a
}

pub fn factors(input:usize) -> Vec<usize> {
    let mut factors1 = Vec::<usize>::new();
    let mut factors2 = Vec::<usize>::new();
    let mid = (input as f32).sqrt() as usize;
    
    for i in 1..(mid+1) {
        if input%i == 0{
            factors1.push(i);
            if i != input / i {
                factors2.insert(0,input / i);
            }
        }
    }

    factors1.append(&mut factors2);
    factors1
}

pub fn is_prime(input: usize) -> bool {
    if input <= 1 {
        return false;
    }
    if input == 2 {
        return true;
    }
    if input & 1 == 0 {
        return false;
    }

    let sqrt = (input as f64).sqrt() as usize;
    for i in (3..=sqrt).step_by(2) {
        if input % i == 0 {
            return false;
        }
    }

    true
}

pub fn factorial(input: usize) -> usize {
    (1..=input).product()
}

pub fn xor_upto(input: usize) -> usize {
    match input % 4 {
        0 => input,
        1 => 1,
        2 => input + 1,
        _ => 0,
    }
}

pub fn ncr(n: usize, r: usize) -> usize {
    let numerator: usize = ((n-r+1)..=n).product();
    let denominator: usize = (1..=r).product();
    return numerator / denominator
} 
```
