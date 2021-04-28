
# [Tamarin-prover] A protocol for secure verification of watermarks embedded into Machine Learning models

This is a project to formally prove the protocol modeled in: 

## Getting Started


### Prerequisites

Requirements for Tamarin-prover 

- [Tamarin-prover manual](https://tamarin-prover.github.io/)

- [Tamarin-prover git](https://github.com/tamarin-prover/tamarin-prover)

## Tamarin Introduction
Tamarin prover is a formal verification tool for cryptographic protocols.
We will introduce the syntactic base but more can be found in the Tamarin-prover Manual as well as many examples in the tamarin git.

A protocol is a set of rules. And to verify security properties of the protocol we use lemmas.

A rule is a link state(s):
~~~~
	rule Rule_Name:
	[State1(parameter1, parameter2), State2(parameter3)]
	--[ActionFact()]->
	[State3(parameter2)]	
~~~~
As we can see a Rule can be applied only if the State1 and State2 are presents. Then it consumes them to create a new state: State3.
Here we can see another thing: ActionFact(). Action facts may be used in two different cases that we will see : Restriction and Lemmas.

To gain in clarity you can use 'let in' :
~~~~
	rule Rule_Name:
		let
		new_param = <parameter1, parameter2>
		in
		[State1(parameter1, parameter2), State2(parameter3)]
		--[ActionFact()]->
		[State3(new_param)]
~~~~


Now, the soul of a protocol is 'exchange'. To exchange on the network, tamarin has two built-ins function : `Out` and `In`.
In can only be in the Initial States since it the entry to receive a message from the network. 
Contrarily Out can only be in the created states since it is a message that will be send to the network. 
Note that everything that is going in the network can be seen, removed, changed by an adversary.

Sometime in a protocol it is needed to restrict something, for example verifying a signature or an Hmac. 
For this there are things called restriction in tamarin:
~~~~
	restriction Equal : "All a b #i. Eq(a,b)@#i ==> a=b"
~~~~
This mean that if there is `Eq(parameter1, parameter2)` in the ActionFact set, then parameter1 has to be equal to parameter2.


We have our set of rule that model our protocol. We now want to prove some security properties for this protocol, therefore we will use lemmas.
Let's first talk about the syntax. It is mostly a mathematical syntax where :
~~~~
	All := For All
	Ex  := There Exist
~~~~
a basic example :
~~~~
	lemma Lemma_Name:
		"All x #i. ActionFact(x)@#i ==> not(Ex #k.KU(x)@#k)"
~~~~
`#i` model a timestamp (if the is ActionFact(x) at time #i).

`K(x)` model that the adversary has knowledge of K.

`KU(x)` model that the adversary sent x to the network (and therefore has knowledge of it).
## SPTHY Files

There are two spthy files:

`corr_watermark.spthy` is the file to verify the execution and the wellformedness of the tamarin model.

`conf_watermark.spthy` is the file in which we verify the confidentiality of the parameters.

## Running the tests

To run the verification do:
	
	tamarin-prover --prove file.spthy
Note that doing all the lemmas at the same time cost ressources. To prove each lemma individually you can do :
	
	talarin-prover --prove=lemma_name file.spthy
You can have an interactive verification :
	
	tamarin-prover interactive file.spthy
For additional parameters, find them with :
	
	tamarin-prover --help

### Output files

	outout_name.txt
The files with this name are the output of the verification of the theory name.spthy



## Authors of the paper

  - **Katarzyna Kapusta** - 
  - **Vincent Thouvenot** - 
  - **Hugo Beguinet** - 
  - **Hugo Senet** - 
  - **Olivier Bettan** - 

