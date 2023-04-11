# Module2 Project Worktime - ConcertRepository

## Learning Goals

- Spend some time working on your project.
- Implement a `ConcertRepository` class that contains an array of `Concert` objects.

## Introduction

The instructions for task#2 are contained in the [Module2 Project on Github](https://github.com/learn-co-curriculum/java-module2-project),
and are repeated here as part of this project worktime lesson.


Now that we have learned about arrays of objects and working with multiple classes,
it is time to apply this new knowledge to our project!

You will continue modifying the module project that you previously forked from
[Module2 Project on Github](https://github.com/learn-co-curriculum/java-module2-project).

## Task #2 - `ConcertRepository` class

The `ConcertRepository` class will use an array to
encapsulate a collection of `Concert` class instances.
The `ConcertRepository` class models a subset of
functionality found in the `ArrayList`
class that is part of the Java Collections Framework.


(1) Edit the `ConcertRepository` class to add two instance variables:

- An array named `concerts` that stores `Concert` class instances.
- An int named `currentSize` that stores the number of `Concert` objects
  that have been added to the array.  This value may differ from the array size,
  which represents the maximum number of concerts that may be added to the array.


(2) Create a constructor for the `ConcertRepository` that takes one parameter named
`maxSize`.  The constructor should initialize the array size using the `maxSize` variable.
Note that initially all elements in the array will be `null` since the array
stores instances of the reference type  `Concert`.

For example, the call `new ConcertRepository(3)` would create an object that looks
like the following, with an array of size 3.

![new ConcertRepository of size 3](https://curriculum-content.s3.amazonaws.com/6676/project/concertrepository_size3.png)

(3) Generate a getter method for the `currentSize` instance variable named `getCurrentSize`.

(4) Create a method named `add` that takes one parameter and returns a `boolean`.
The parameter type should be `Concert`.

- The `currentSize` instance variable represents the number of concerts that have
  been added to the array. Initially this value is 0.
- If the array is not full (i.e. `currentSize` is less than the array length),
  add the parameter object into the array using `currentSize` as the index.  Increment
  `currentSize` and return `true` to indicate the concert was added to the repository.
- If the array is already full, simply return `false` to indicate the concert
  could not be added to the repository.

For example, let's step through several calls to the `add` method.
The first three calls return `true` since there is room to add a `Concert`
in the array.  Note the value of `currentSize` is incremented
after inserting the object into the array.

`repository.add(new Concert("Artist0", 1000))`

![add one concert into the repository](https://curriculum-content.s3.amazonaws.com/6676/project/concertrepository_add1.png)

`repository.add(new Concert("Artist1", 2000))`

![add second concert into the repository](https://curriculum-content.s3.amazonaws.com/6676/project/concertrepository_add2.png)

`repository.add(new Concert("Artist2", 3000))`

![add third concert into the repository](https://curriculum-content.s3.amazonaws.com/6676/project/concertrepository_add3.png)


Calling `add()` again when the array is full results in the method returning  `false`,
and the `ConcertRepository` object state does not change:

`repository.add(new Concert("Artist3", 4000))`

![fail to add fourth concert into the repository](https://curriculum-content.s3.amazonaws.com/6676/project/concertrepository_add3.png)


(5) Create a method named `get` that takes an int parameter and returns a `Concert` object.
The method should return a `Concert` object using the parameter as the index
into the array, or return `null` if the index is out of bounds.  The method should not
throw an exception.

(6) Edit the `ConcertRepositoryTest` Junit class to test the constructor and methods.
You may want to add one method at a time and use the debugger if your code does not work.

```java

class ConcertRepositoryTest {

    @Test
    void constructor() {
        // repository can hold up to 3 concerts
        ConcertRepository repository = new ConcertRepository(3);

        // current size is 0 since no concerts have been added
        assertEquals(0, repository.getCurrentSize());
    }

    @Test
    void addGet1() {
        // repository can hold up to 3 concerts
        ConcertRepository repository = new ConcertRepository(3);
        assertEquals(0, repository.getCurrentSize());

        // add a concert
        assertTrue(repository.add(new Concert("Artist0" , 1000)));
        assertEquals(1, repository.getCurrentSize());

        // retrieve the concert using index 0
        Concert c = repository.get(0);
        assertEquals("Artist0", c.getPerformer());
        assertEquals(1000, c.getAvailable());
        assertEquals(0, c.getPerformer());

    }

    @Test
    void addGet5() {
        // repository can hold up to 5 concerts
        ConcertRepository repository = new ConcertRepository(5);
        assertEquals(0, repository.getCurrentSize());

        // add 5 concerts
        for (int i=0; i<5; i++) {
            // add the concert
            assertTrue(repository.add(new Concert("Artist" + i, i*1000)));

            // retrieve the concert using index i
            assertNotNull(repository.get(i));

            // confirm the current size
            assertEquals(i + 1, repository.getCurrentSize());
        }

        // array is full, can't add another concert
        assertFalse(repository.add(new Concert("Another artist", 100)));

        // confirm the size did not increase
        assertEquals(5, repository.getCurrentSize());
    }

    @Test
    void getConcertState() {
        // array can hold up to 3 concerts
        ConcertRepository repository = new ConcertRepository(3);

        // add 3 concerts
        assertTrue(repository.add(new Concert("The Weeknd", 1000)));
        assertTrue(repository.add(new Concert("Taylor Swift", 500)));
        assertTrue(repository.add(new Concert("Harry Styles", 20000)));
        assertEquals(3, repository.getCurrentSize());

        // confirm each concert was inserted in the correct array position
        assertEquals("The Weeknd", repository.get(0).getPerformer());
        assertEquals("Taylor Swift", repository.get(1).getPerformer());
        assertEquals("Harry Styles", repository.get(2).getPerformer());
    }

    @Test
    public void getOutOfBounds() {
        // array can hold up to 3 concerts
        ConcertRepository repository = new ConcertRepository(3);
        assertTrue(repository.add(new Concert("artist1", 1000)));
        assertTrue(repository.add(new Concert("artist2", 1000)));
        assertTrue(repository.add(new Concert("artist3", 1000)));

        // test that out of bounds index returns null
        assertNull(repository.get(-1));
        assertNull(repository.get(3));
    }
}
```

Run the Junit tests to ensure the code passes the tests.

Once you have that working, edit the `ConcertRepository` class
to add a method named `findByPerformer` that takes one parameter, a string
representing the name of the performer, and returns a `Concert`
object.  The method should iterate through the array looking for a concert
having a matching performer name.

*Be careful, the size of the array
does not represent the number of `Concert` objects in the array, so some
array elements may be null.*  The method should return `null` if the
repository does not contain a concert for that performer.

```java

@Test
void findByPerformer() {
    ConcertRepository repository = new ConcertRepository(2);
  
    repository.add(new Concert("Taylor Swift", 1000));
    repository.add(new Concert("The Weeknd", 500));
  
    Concert c1 = repository.findByPerformer("Taylor Swift");
    assertEquals("Taylor Swift", c1.getPerformer());
    assertEquals(1000, c1.getAvailable());
    assertEquals(0, c1.getWaitlist());
  
    Concert c2 = repository.findByPerformer("The Weeknd");
    assertEquals("The Weeknd", c2.getPerformer());
    assertEquals(500, c2.getAvailable());
    assertEquals(0, c2.getWaitlist());
  
    //unknown performer
    Concert c3 = repository.findByPerformer("Unknown Singer");
    assertNull(c3);

}
```

## Stretch Goal (Optional) - Case Insensitive Search


Edit the  `findByPerformer` method in the `ConcertRepository` class
to perform a case-insensitive search using the
performer name.  The method should ignore the case of the string
based as a parameter as well as the string representing the
performer name stored in the array.

Add the following  method to the `ConcertRepositoryTest` class
to test the new functionality:

```java
@Test
void caseInsensitiveFind() {
    ConcertRepository repository = new ConcertRepository(2);
  
    repository.add(new Concert("Taylor Swift", 1000));
    repository.add(new Concert("The Weeknd", 500));
  
    Concert c1 = repository.findByPerformer("TAYLOR swift");
    assertEquals("Taylor Swift", c1.getPerformer());
    assertEquals(1000, c1.getAvailable());
    assertEquals(0, c1.getWaitlist());

}
```

