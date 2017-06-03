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

     (II) get cars from street

This line basically contains the same information as in `(I)`, and it would even allow us to refer to the variable `passengers` later on.
Only the syntax doesn't seem to be very good, as `bus` is an object here, but modelling it as a subject seems to be more natural, as in `bus gives passengers`.
The only problem with that is the use of conjugation.
If we could just use the infinitive form of the verb...

    (III) bus: give passengers

Sounds good!
Short, precise, free of redundancies.
Statement `(III)` would command the program to let the bus do whatever it needs to `give passengers` (method call), and store that information for later use as `passengers` (variable binding).
It would also allow the compiler to do type checking, as he nows that `passengers` can only be a collection of `Passenger` instances.
Otherwise, it would've been named differently.

Now, in `(I)`, we would've declared and implemented the behavior in class `Bus`:

    (IV) class Bus { List<Passenger> getPassengers() { /* ... */ } }

We can do the same with a syntax as in `(III)`.

    (V) class Bus { give passengers { /* ... */ } }

But often, we restrict or describe the information we want.

    (VI) List<Passenger> passengersWithLuggage = bus.getPassengersWithLuggage();
    (VII) List<Passenger> passengersWithLuggage = bus.getPassengers(ServiceType.LUGGAGE);

Also, we modify variable names to describe what we're doing.

    (VIII) List<Passenger> passengersToNotify = bus.getPassengers();

Both modifies the variables name and is reused later on when accessing the information.
It is also often used to distinguish between different data.

    (IX) bus: give passengers #withLuggage
         bus: give passengers #withoutLuggage
         capacityChecker: validate passengers #withLuggage
         database: store passengers #withoutLuggage
