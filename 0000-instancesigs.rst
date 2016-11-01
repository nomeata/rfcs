- Feature Name: Standardize `InstanceSigs`
- Start Date: 2016-01-01
- RFC PR:
- Haskell Report Issue:



#######
Summary
#######

GHC's ``InstanceSigs`` extension allows method definitions in instance declarations sport type signatures, which aids teaching, documentation and general consistency of the language. It should be enabled by default.


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

See https://downloads.haskell.org/~ghc/latest/docs/html/users_guide/glasgow_exts.html#ghc-flag--XInstanceSigs


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

If people really feel the need to that there needs to be something to be discussed for the sake of having an discussion, one might discuss the choice of GHC to allow more general types that required by the instance.
