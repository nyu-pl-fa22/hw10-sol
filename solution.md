## Problem 1:

1. Static type: A. Dynamic Type: B. The dynamic type is B because a1
   is initialized with a freshly created `B` instance.

2. Static type: A. Dynamic Type: A. The static type of `a1` is
   `A`. Since `m1` is a private method, it is not dynamically
   dispatched. So the call goes to `A.m1` which returns a freshly
   created `A` instance.

3. Static type: A. Dynamic Type: B. The dynamic type of `a1` is
   `B`. Since `m3` is virtual it is dynamically dispatched based on
   the vtable of `B`. `B` inherits `m3` from `A` so to the call goes
   to `A.m3`. The method returns `this` which coincides with
   `a1`. Thus, the dynamic type of `a3` is `B`.

4. The call is dispatched to `A.m1`. See the explanation for 2.

5. The call is dispatched to `A.m2`. This is because the dynamic type
   of a2 is `A` as explained in 2.

6. The call is dispatched to `A.m3`. See the explanation for 3.

7. The call is dispatched to `B.m2`. This is because `m2` is virtual
   and the dynamic type of `a3` is `B`. Hence, the call is dispatched
   based on the vtable of `B` which overrides `m2` with `B.m2`.


## Problem 2:

1. Data layout of `Base`:

   ```
   Base.name: String
   Base.x: Base
   ```

   Data layout of `Derived`:
   
   ```
   Base.name: String
   Base.x: Base
   Derived.name: String
   Derived.z: Int
   ```

2. VTable of `Base`

   ```
   Base.toString(): String
   Base.m1(AnyRef): AnyRef
   Base.m2(): Base
   ```
   
   VTable of `Derived`

   ``` 
   Base.toString(): String
   Derived.m1(AnyRef): Derived
   Base.m2(): Base
   Derived.m4(Derived): String
   ```
   
3. The dynamic type of this expression is `Derived`. This is because
   the call to `m2` is dispatched to `Base.m2` which returns the value
   stored in `this.x`, which is `this`. Since the dynamic type of
   `this` is `Derived` for this call, so is the dynamic type of
   `this.x`. Hence, `m2` returns the reference to the `Derived`
   instance created by the `new` expression.

4. As we saw in 3., the dynamic type of the object on which `m2` is
   called is `Derived`. According to the vtable of `Derived`, the call
   goes to `Base.toString`. `Base.toString` returns the value stored
   in `Base.name` in the data layout of `Derived`. Since `name` is
   declared private in `Base`, the redeclaration of `name` in
   `Derived` does not override the declaration of `Base.name`. Hence,
   `Base.name` still stores the value `Base` in the `Derived`
   instance and the call evaluates to `Base`.

   If we override the definition of `toString` in `Derived` as
   indicated, then the call to `toString` will be dispatched to the
   new implementation because the dynamic type of the object returned
   by `m2` is `Derived`. In the new implementation of `toString`, the
   field `name` now refers to `Derived.name` which stores the value
   `Derived`. Hence, the whole expression now evaluates to `Derived`.
