# Precise Code

An idea about a programming language which allows you to code more precisely and with less redundancy.

Maybe something great. Maybe just some playin' around.

## Basic idea

In our applications, a huge amount of LOC are about transporting and transforming data between objects.

When I look closer at our Java code, I often detect redundancies.
In an assignment, the name of the type repeats itself in the variable name on the left hand side, and in the method's name on the right hand side:

    (I) List<Passenger> passengers = bus.getPassengers();
             ^^^^^^^^^  ^^^^^^^^^           ^^^^^^^^^

This is because all of those names came from a common model.
A type is named after a real-world object which he represents.
A variable is named after the objects it contains, often with descriptions or limitations.
Method names compose from a verb and an object, often also with further descriptions.

A language that avoids such redundancies would allow the compiler to actually _understand_ these names.

     (II) get passenger_s from bus

This line basically contains the same information as in `(I)`:

* We want to `get passenger_s` from an object called `bus`.
  This is the same as the method invocation `bus.getPassengers()`.
* In following code, we know the return value as `passenger_s`.
  This is the same as the assignment to the variable `passengers`.
* As soon as we know what a `Passenger` is, and we also define how to store multiple of them,
  we know that a variable called `passenger_s` (with trailing `s` as plural suffix) may contain multiple `Passenger` instances.
  This is the same as declaring the variable's type as `List<Passenger>`.

Note, that we decide to use `passenger_s` instead of `passengers`, that is separating the plural suffix from the root word, because language tends to be ambigous with prefixes and suffixes.

Now, while we could be quite satisfied with `(II)` being a much more concise version of `(I)`, carrying the same set of information, there's still some room for improvements.
The syntax doesn't seem to be very good, as `(II)` uses `bus` as an object without stating which _subject_ is getting the passengers here.
Modelling the `bus` as a subject seems to be more natural and allows to be even more concise, as in `bus gives passengers`.
The only problem with that is the use of conjugation.
Lucky enough, the verb in imperative form in present active suffers no conjugation.

    (III) bus: give passenger_s

Sounds good!
Short, concise, free of redundancies.
Statement `(III)` would command the program to let the bus do whatever it needs to `give passenger_s` (method call), and store that information for later use as `passenger_s` (variable binding).
It would also allow the compiler to do type checking, as he nows that `passenger_s` can only be a collection of `Passenger` instances.
Otherwise, it would've been named differently.

Let's briefly look at some further examples.

    (IV) database: find book_s
    (V) mailServer: send mail
    (VI) soundDevice: play musicStream

**TODO:** Here we have the problem of being unable to distinguish input from output parameters.

Now, in `(I)`, we would've declared and implemented the behavior in class `Bus`:

    (VII) class Bus { List<Passenger> getPassengers() { /* ... */ } }

We can do the same with a syntax as in `(III)`.

    (VIII) class Bus { give passenger_s { /* ... */ } }

But often, we restrict or describe the information we want.

    (IX) List<Passenger> passengersWithLuggage = bus.getPassengersWithLuggage();
    (X) List<Passenger> passengersWithLuggage = bus.getPassengersWith(ServiceType.LUGGAGE);

Also, we modify variable names to describe what we're doing.

    (XI) List<Passenger> passengersToNotify = bus.getPassengers();

Both modifies the variables name and is reused later on when accessing the information.
It is also often used to distinguish between different sets of data.
In our language, we can use _hashtags_ to identify the correct method call.
They'll stick to the result variables.

    (XII) bus: give passenger_s #withLuggage
          capacityChecker: validate passenger_s #withLuggage

Here, we declare `passenger_s #withLuggage` in the first line, and use it in the second.
We allow the author to reuse the arguments in the variable name, which especially makes sense when we have different sets of objects from the same type.

    (XIII) bus: give passenger_s #withEMail
           bus: give passenger_s #withoutEMail
           mailService: notify passenger_s #withEMail
           customerService: call passenger_s #withoutEMail

