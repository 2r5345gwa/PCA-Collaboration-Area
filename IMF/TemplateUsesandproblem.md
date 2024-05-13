# The IMF Template, its uses and the problem it solves.

The term IMF Template is used in various other texts and it is used to describe various different concepts. We want to collect and unify those definitions and the uses that are attributed to the thing "IMF Template" in this text.

An IMF Type describes what characterizes a kind of IMF Element, for example this block has Attribute A and these two terminals, therefore it is an instance of Type X or I can classify this Block as Type X.

An IMF Template does not have a use when it comes to classification and nothing is an instance of an IMF Template. An IMF Template is a tool one uses when doing the work of instantiating.

The main feature of an IMF Template is to reduce or minimize the amount of repetitive work someone who is creating an IMF Model has to do.

One of the ways an IMF Template enables this is by supplying pre-set values for specific attributes for a given block.

## The fuse box

Lets work our way trough an example.

Lets imagine that we have a fuse box, the fuse box has eight rails, each rail has 8 breakers and each breaker is rated for at least 20 amperes.

The fuse box type only specifies that it should contain one or more rails, the alternative results in fuse-box-1-rail, fuse-box-2-rails types and so on.

The rail type only specifies that it acts as a mounting for one or more breakers or similar equipment. Again the alternative results in rail-4breakers-and-single-meter, rail-4breakers, rail-5breakers-and-two-meters types and so on.

The breaker type only specifies that it needs a specific amper tolerance, we don't want a type for breakers with 10, 12, 16, 20, and so on.

The specific work we want to spare the modeller for is having to input the number 20, 64 times for each fuse box they want in their model. This is where the IMF Template does it magic.

An IMF Template that helps us out in this case specifies that we have a fuse box template, and that fuse box template specifies that each fuse box contain 8 rails, and each rail acts as a mount for 8 breakers with a tolerance of 20 amperes.

## What does this mean our many types and templates?

In short, if you are going to be using a tool that directly creates IMF Models and those models many very similar yet complex parts, you will want templates in order to not do enormous amounts of manual repetitive labour.

On the other hand, if you are a developer and you're tasked with exporting from a source system and correctly classifying the entities in the export, templates are not the tool you need, then you need Types which are what we use for classification.

## An Implementation of IMF Template

Currently there is no implementation in the PCA tech stack that meets the requirements of the IMF Template.

The IMF Manual does not specify a concept or a process to achieve the function attributed to IMF Template here either.