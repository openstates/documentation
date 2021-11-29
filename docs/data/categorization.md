Categorization
==============

One of the ways that we add value to the data we provide is by
attempting to classify bills, actions, and votes across states.

This allows us to let states use their own names for these things, but
for us to try to provide some mapping to a common simplified view of the
legislative process.

Bill Types
----------

State legislatures deal with more than bills. Despite the name of the
bill objects in our data we take in all types of legislation that a
state might produce. Generally looking at the bill_id will help you
determine the type of legislation, but to make things easier across
states we provide a type field on bills. This field is a list with one
(or more) of the following values:

Common Values:

-  bill
-  resolution
-  joint resolution
-  concurrent resolution
-  constitutional amendment

Some states also make use of additional types such as 'contract',
'nomination', 'memorial' and more.

Action Types
------------

Although most states follow very similar parlimentary procedure the
names that their bill status systems use for various actions almost
never match up. To make analysis and the building of certain types of
tools easier we attempt to classify common actions. In using our data
you'll find these values in the classification field of actions.

-  filing - bill is filed (where this is a separate action from
   introduction)
-  introduction - introduced, typically the first action
-  reading-1 - first reading (often same as introduction)
-  reading-2 - second reading
-  reading-3 - third reading (often same as passage)
-  passage - bill is passed by the chamber
-  failure - bill fails to proceed from the chamber
-  withdrawal - bill is withdrawn
-  substitution - a substitution is made to the bill text
-  deferral - consideration of the bill was deferred
-  receipt - a bill was received by another chamber
-  referral - a bill was sent somewhere for consideration
-  referral-committee - a bill was sent to a committee for consideration
-  became-law - the bill became law (through signature or inaction)
-  amendment-introduction - an amendment is introduced
-  amendment-passage - an amendment passes
-  amendment-withdrawal - an amendment is withdrawn
-  amendment-failure - an amendment fails to pass
-  amendment-amendment - an amendment is amended
-  amendment-deferral - consideration of an amendment is deferred
-  committee-passage - the bill passes the current committee (unknown
   outcome, typically favorble)
-  committee-passage-favorable - the bill passes the current committee
   favorably
-  committee-passage-unfavorable - the bill passes the current committee
   with an unfavorable report
-  committee-failure - the bill fails to advance out of committee
-  executive-receipt - the bill is sent to the governor
-  executive-signature - the governor signs the bill
-  executive-veto - the governor vetos the bill
-  executive-veto-line-item - the governor uses a line-item veto to
   strike part of a bill
-  veto-override-passage - a veto override vote occurred and succeeded
-  veto-override-failure - a veto override vote occurred and failed

Vote Types
----------

Similarly to actions, we make an effort to categorize the motion being
voted upon. You'll find these values in the categorization field on
VoteEvents.

Possible values:

-  **bill-passage** - This is a vote to pass (either out of committee or
   a chamber)
-  **amendment-passage** - Vote on amending a bill
-  **veto-override** - Vote to override an executive veto
