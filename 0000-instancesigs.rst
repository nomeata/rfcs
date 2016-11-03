- Feature Name: Allow type signatures in instance declarations
- Start Date: 2016-01-01
- RFC PR:
- Haskell Report Issue:


#######
Summary
#######

Method definitions in instance declarations should be able to have type signatures, just like top-level and local definitions. This aids teaching, documentation and general consistency of the language.


##########
Motivation
##########


As a teacher who uses live coding in a text editor, I usually write out
all type signatures. Not being able to do that for instance methods is
a minor annoyance, and enabling extension is quite annoying in a
beginner/teaching settings (compared to a, say, industrial development
settings).

The implementation effort of this is very small (already done in GHC.).

Also see http://stackoverflow.com/questions/8367426/why-cant-one-put-type-signatures-in-instance-declarations-in-haskell.


###############
Detailed design
###############


In Haskell 98, you can't write a type signature in an instance declaration,
but it is sometimes convenient to do so. With this proposal, you can do so. For
example: ::

      data T a = MkT a a
      instance Eq a => Eq (T a) where
        (==) :: T a -> T a -> Bool   -- The signature
        (==) (MkT x1 x2) (MkTy y1 y2) = x1==y1 && x2==y2

Some details

-  The type signature in the instance declaration must be more
   polymorphic than (or the same as) the one in the class declaration,
   instantiated with the instance type. For example, this is fine: ::

         instance Eq a => Eq (T a) where
            (==) :: forall b. b -> b -> Bool
            (==) x y = True

   Here the signature in the instance declaration is more polymorphic
   than that required by the instantiated class method.

-  The code for the method in the instance declaration is typechecked
   against the type signature supplied in the instance declaration, as
   you would expect. So if the instance signature is more polymorphic
   than required, the code must be too.

-  One stylistic reason for wanting to write a type signature is simple
   documentation. Another is that you may want to bring scoped type
   variables into scope. For example: ::

       class C a where
         foo :: b -> a -> (a, [b])

       instance C a => C (T a) where
         foo :: forall b. b -> T a -> (T a, [b])
         foo x (T y) = (T y, xs)
            where
              xs :: [b]
              xs = [x,x,x]

   If the `ScopedTypeVariables` extension is enabled (or the default behaviour
   in a future version of the report), then the ``forall b`` scopes over the
   definition of ``foo``, and in particular over the type signature for ``xs``.


The above text was taken from the documentation of GHC’s language extension ``InstanceSigs``. Given the trivial and clean-up like nature of this change, this propsoal does not propose to add an extension, but simply make this behaviour part of the standard, without defining it as a language extensions. Implementors are still  free to implement a ``NoIntanceSigs`` extensions if they choose to.

#########
Drawbacks
#########

Cannot think of any.

############
Alternatives
############

Work-around: The user can define the method as a top-level function with type signature and put ``foo = theRealFoo`` in the instance declaration.

####################
Unresolved questions
####################

None
