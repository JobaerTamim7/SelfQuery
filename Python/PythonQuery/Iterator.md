## What is Iterator and Iterable?

### Iterator:

An **iterator** is an object that enables you to traverse through all the elements of an iterable, one at a time. It keeps track of where it is in the sequence. It has the following characteristics:

- An iterator implements the `__next__()` method, which returns the next element in the sequence.
- Once there are no more elements, it raises the `StopIteration` exception.
- An iterator must also implement the `__iter__()` method, which returns the iterator object itself.

In simple terms, an iterator is like a "pointer" that tracks your position as you move through the sequence.

**Example of an Iterator:**

```python
# Creating a simple iterator
my_list = [1, 2, 3]
my_iter = iter(my_list)  # Creates an iterator from the list

print(next(my_iter))  # Output: 1
print(next(my_iter))  # Output: 2
print(next(my_iter))  # Output: 3
# print(next(my_iter))  # Raises StopIteration because we have reached the end
```

### Iterable

An **iterable** is the class that has the `__iter__()` method, which returns an iterator. Any object that can be looped over (like lists, tuples, strings, sets, etc.) is considered an iterable.
_More like the question "Is list iterable (class) or not?"_

- The **`__iter__()`** method returns an iterator object.
- The iterator object implements the `__next__()` method to retrieve the next item in the sequence.
- Common built-in iterables include lists, tuples, sets, strings, and dictionaries.

**Example of an Iterable:**

```python
my_list = [1, 2, 3]
for item in my_list:  # The list is an iterable
    print(item)
```

```python
class CountDownByTwo:
    def __init__(self, start):
        self.current = start

    def __iter__(self):
        """
        __iter__ method make a class iterable
        """
        return self

    def __next__(self):
        if self.current < 0:
            raise StopIteration #raises the exception to stop iteration
        else:
            number = self.current
            self.current -= 2
            return number


```
