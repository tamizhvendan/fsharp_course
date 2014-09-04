### Why Should You Learn Functional Programming

#### Why I Started Learning Functional Programming

Before sharing why you should learn functional programming let me share why I started learning it.

Two years back (in the early 2012) when I started learning `LINQ` in `C#` I came across the word called **Functional Programming**. 
To be honest it took me quite some time to understand how functional programming works in the context of `LINQ`. 
While coding in `LINQ`, I have started realizing the beauty and expressiveness of functional way of programming. 

After seeing the benefits of `LINQ` in my day to day coding, I've started researching more and more about functional programming and landed in the `Haskell` world! Though I didn't learn it fully, [the book](http://learnyouahaskell.com/chapters) which I've used to learn Haskell has given me more insights about thinking functionally to solve real time problems. 

I am really inspired by this new alternative way of thinking and I've felt that this would really help all the object oriented programmers to become a better programmer. 

#### How Functional Programming Can Help You

If you are using `LINQ` in `C#` or `Lamda Expression` in `java`, you are already getting benefited in terms of less, expressive and bug-free code. 
Apart from this there are lot of other significant benefits in using functional programming.

* Verbs - Buried abstraction in OO World

### Nouns

In Object Oriented Programming, the problem domain has been modeled as various objects(nouns) collaborate with each other in the form of 
messages(verbs). What we are actually doing here is, we are creating abstractions and solves the problems in an organized and cleaner way. But this way
of creating abstractions does not scale in all the scenarios.

Let us see this code instead of boring theory :relieved:

Lets rewind the time machine to travel back to 2006 (.NET Framework version 2.0) which doesn't have `LINQ` support. You are asked to solve the following problem statement
> You are working on a Student Management Applicaiton. Each student is having a name and an age. You are asked to create module to sort the Students based on their name

```c#
public class Student
{
	public int Age { get; set; }
	public string Name { get; set; }
}
```

To sort based on Name we need to write a comparer which compares two student names and return some integer

```c#
public class StudentNameComparer : IComparer<Student>
{
	public int Compare(Student x, Student y)
	{
		return string.Compare(x.Name, y.Name, StringComparison.CurrentCulture);
	}
}
```

Then in the main program we will be doing the sorting using `Array.Sort` method

```c#
class Program
{
    static void Main(string[] args)
    {
        List<Student> students = new List<Student>();
		
		// Populate Students List here	
	
		Student[] studentsSortedByName = students.ToArray();       

        Array.Sort(studentsSortedByName, new StudentNameComparer());
    }
}
```

Hmmm. You might think what is a big deal in it ? But there is! Let me complicate the scenario by adding a new requirement

> You also asked to do sorting based on Age!

How do you do it ?

Well! Just create yet another Comparer which compares the age
```c#
public class StudentAgeComparer : IComparer<Student>
{
    public int Compare(Student x, Student y)
    {
        if (x.Age == y.Age)
        {
            return 0;
        }
        if (x.Age < y.Age)
        {
            return -1;
        }
        return 1;
    }
}
```
```c#
class Program
{
    static void Main(string[] args)
    {
        List<Student> students = new List<Student>();
		
		// Populate Students List here	
	
		Student[] studentsSortedByAge = students.ToArray();       

        Array.Sort(studentsSortedByAge, new StudentAgeComparer());
    }
}
```

So much of code to do the trivial thing, isn't it ?

Let us dig deep to figure out what is wrong here. First let us start with the comparers `StudentAgeComparer` and `StudentNameComparer`.
These comparers (nouns) actually escorting the method `Compare` (verb) and not doing anything productive apart from this. In OO world a verb should
always be associated with a noun and because of this way of abstraction, we are adding an unnecessary complexity to the code. 
Just think of yet another scenario where want to sort the student by rank, customer by name, product by name. We need to write so much comparers!

### Verbs to the rescue

Let us see the student sorting scenario from a different perspective. In this new pair of (functional)eyes we are the same scenario as

* **Order** _Student_ by **Name**  
* **Order** _Student_ by **Age**  
* **Order** _Customer_ by **Name**  
* **Order** _Product_ by **Name**  

i.e the verb **Order** is common across all the scenario. 

The only thing which is changing is _what we are ordering_ and _by which property_. 
Let us model things based on this verb abstraction.

PsudoCode
```
OrderBy<Student>(s -> s.Name)
OrderBy<Student>(s -> s.Age)
OrderBy<Customer>(c -> c.Name)
OrderBy<Product>(p -> p.Name)
```

Here OrderBy is a function which models the verb (Order). Two things to note

* OrderBy is generic and it can act on any types of objects (Student, Customer, Product,...)
* OrderBy doesn't know which property to choose so, we need to tell explicitly.

So, what we gained because of this ?

Let us go back to the initial requirement

> You are working on a Student Management Applicaiton. Each student is having a name and an age. 
> You are asked to create module to sort the Students based on their name and age

Now change the time machine to land in 2014. We have `LINQ` support in `C#` now which supports verbs (functions) as first class citizens

```c#
class Program
{
    static void Main(string[] args)
    {
        List<Student> students = new List<Student>();

        // Populate Students List here	

        var studentsSortedByName = Enumerable.OrderBy(students, s => s.Name);
        var studentsSortedByAge = Enumerable.OrderBy(students, s => s.Age);
    }
}
```

 
