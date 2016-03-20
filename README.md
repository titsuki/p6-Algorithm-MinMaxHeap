[![Build Status](https://travis-ci.org/titsuki/p6-Algorithm-MinMaxHeap.svg?branch=master)](https://travis-ci.org/titsuki/p6-Algorithm-MinMaxHeap)

NAME
====

Algorithm::MinMaxHeap - double ended priority queue

SYNOPSIS
========

    use Algorithm::MinMaxHeap;

    # item is a Int
    my $heap = Algorithm::MinMaxHeap.new();
    $heap.insert(0);
    $heap.insert(1);
    $heap.insert(2);
    $heap.insert(3);
    $heap.insert(4);
    $heap.insert(5);
    $heap.insert(6);
    $heap.insert(7);
    $heap.insert(8);

    $heap.find-max.say # 8;
    $heap.find-min.say # 0;

    my @array;
    while (not $heap.is-empty()) {
	    @array.push($heap.pop-max);
    }
    @array.say # [8, 7, 6, 5, 4, 3, 2, 1, 0]

    # item is a class

    # sets compare-to method using Algorithm::MinMaxHeap::Comparable role
    my class State {
       also does Algorithm::MinMaxHeap::Comparable[State];
       has Int $.value;
       has $.payload;
       submethod BUILD(:$!value) { }
       method compare-to(State $s) {
   	        if (self.value == $s.value) {
   	           return Order::Same;
   	        }
   	        if (self.value > $s.value) {
   	           return Order::More;
   	        }	      
   	        if (self.value < $s.value) {
   	           return Order::Less;
   	        }
       }
    }

    # specify Algorithm::MinMaxHeap::Comparable role as an item type
    my $class-heap = Algorithm::MinMaxHeap.new(type => Algorithm::MinMaxHeap::Comparable);
    $class-heap.insert(State.new(value => 0));
    $class-heap.insert(State.new(value => 1));
    $class-heap.insert(State.new(value => 2));
    $class-heap.insert(State.new(value => 3));
    $class-heap.insert(State.new(value => 4));
    $class-heap.insert(State.new(value => 5));
    $class-heap.insert(State.new(value => 6));
    $class-heap.insert(State.new(value => 7));
    $class-heap.insert(State.new(value => 8));

    $class-heap.find-max.value.say # 8;
    $class-heap.find-min.value.say # 0;

    my @array;
    while (not $class-heap.is-empty()) {
	    my $state = $class-heap.pop-max;
	    @array.push($state.value);
    }
    @array.say # [8, 7, 6, 5, 4, 3, 2, 1, 0]

DESCRIPTION
===========

Algorithm::MinMaxHeap is a simple implementation of double ended priority queue.

CONSTRUCTOR
-----------

    my $heap = Algorithm::MinMaxHeap.new(); # when no options are specified, it sets type => Int implicitly
    my $heap = Algorithm::MinMaxHeap.new(%options);

### OPTIONS

  * `type => Algorithm::MinMaxHeap::Comparable|Cool|Str|Rat|Int|Num`

Sets either one of the type objects which you use to insert items to the queue.

METHODS
-------

### insert($item)

    $heap.insert($item);

Inserts an item to the queue.

### pop-max()

    my $max-value-item = $heap.pop-max();

Returns a maximum value item in the queue and deletes this item in the queue.

### pop-min()

    my $min-value-item = $heap.pop-min();

Returns a minimum value item in the queue and deletes this item in the queue.

### find-max()

    my $max-value-item = $heap.find-max();

Returns a maximum value item in the queue.

### find-min()

    my $min-value-item = $heap.find-min();

Returns a minimum value item in the queue.

### is-empty() returns Bool:D

    while (not $heap.is-empty()) {
	         // YOUR CODE
    }

Returns whether the queue is empty or not.

### clear()

    $heap.clear();

Erases all items in the queue.

CAUTION
=======

Don't insert both numerical items and stringified items into the same queue.

It will cause mixing of lexicographical order and numerical order.

AUTHOR
======

titsuki <cookbook_000@yahoo.co.jp>

COPYRIGHT AND LICENSE
=====================

Copyright 2016 titsuki

This library is free software; you can redistribute it and/or modify it under the Artistic License 2.0.

This algorithm is from Atkinson, Michael D., et al. "Min-max heaps and generalized priority queues." Communications of the ACM 29.10 (1986): 996-1000.
