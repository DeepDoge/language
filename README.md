# language

this repo is a dump for my ideas about a programming language in my mind, that i havent created yet.

this is gonna be more about syntax, and typing system. mostly, 
but the language should be able to compile to machine code, 
and shouldnt have default memory management. 
it can enforce memory safety using the language itself like rust does, but its not as complex, and as flexable.

so the main thing about types is, each variable definition will have two types, the getter type and setter type.

what do i mean by that?
```ts
var foo = "hello"
// getter: "hello"
// setter: string
```
so i can set any `string` to `foo`, but currently in the current context its `"hello"`.

for example:
```ts
var n: i64 = 123
// getter: 123
// setter: i64

n = 321
// getter: 321
// setter: i64

n = randomI64()
// getter: i64
// setter: i64

if (n == 111) {
   // getter: 111
   // setter: i64
}

if (n >= 10) {
   // getter: 10..
   // setter: i64
   if (n == 1) {
      // unreachable code (darkened)
   }
}
```

etc, as you can see it can become really complex really quickly. but we have to solve this, make it work.

but if we can handle all of the cases for every primitive operation, it should JUST work.

when it comes to functions, function return type shouldn't decide the what function gives when called.

we should decide what the function is returning by kinda inlining it with its arguments, so we know exactly what it might return for any given arg.

typeing system exists, and it should be flexable like typescript, but not 1:1 because we should be more sure while giving answers. i think css and css functions are great examples of how a type system might work.

type system is powerful so we dont need codegeneration for typing. but it should be more stable and direct than typescript.

when it comes to types and structures, we should go with compositions. combining multiple types into one.

but also we should be able to check brand of shape of types at the same time. some how. gotta think about it.

it should be possible:
```ts
val is Foo
```

we dont nneed impl methods btw, instead our functions can use `self` arg name as their first arg. then language can auto detect what connects to what.

i believe we can make our variables immutable by default? or not? or args are immutable by default only? idk.

i think immutable by default is the way. we can use the `mut` keyword to make thing mutable.

also we dont like using symbols, everyhing should have keyword, for example a pointer can be defined with the `ref` keyword, similar to C# `ref`.

so example.
```ts
func foo(arg: ref Foo) // arg is immutable ref to immutable Foo
func foo(mut arg: ref Foo) // arg can be changed to point to another Foo as a variable, but it cant modify the Foo its pointing to.
func foo(arg: ref mut Foo) // arg cant be changed as variable, but Foo its pointing at can be modified.
func foo(mut arg: ref mut Foo) // arg is mutable, Foo is also mutable.
```
so as you can see syntax is simple.

language doesnt have keywords reserved for the most part, for example i should be able to do `var var = "abc"`, language should know what im doing based on the context. (i did this before)

no error throws, not even panics. everything must give an error. but we dont have result pattern or anything weird like that. we just return the error, so its in the return type union.

you can either do:
```ts
var result = foo()
if (result is Error) {
   return result
}
```

we can also have some useful syntax stuff like:
```ts
var result = foo()! // if it gives error returns it immediately.

// also works with any variable
var result = foo()
foo! // or
bar(foo!) // or
log(foo!.name) // etc
```

it just excludes the error types and returns the current function for them immediately. instead of hand writing if statements.
these errors has to be handled at some point because `!` can only be used inside a function.
you just decide what should handle it at last by moving the error higher everytime.

no `dynamic`, `any` or anything like that ever no where in the codebase. thats just unsafe. never give opportunity to fuck up types. give errors.

more about typing, it still similar to typescript but as i said better and more stable.
for example we can have `i64` or `i32` right. but we can also have `10..` which we dont know float or int or what ever.
so, for example this gets anything gt 10, doesnt care about number type, just like an enum, lsp should give possible types based on number type that exist:
```ts
var foo: 10..
```

now this is more specific. it says its the middle part of the i64 and 10.. groups. etc... you get the idea:
```ts
var foo: i64 & 10..
```
