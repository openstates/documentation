Categorization
==============

One of the ways that we add value to the data we provide is by attempting to categorize bills, actions, and votes across states.

.. _bill-types: 

Bill Types
----------

State legislatures deal with more than bills.  Despite the name of the bill objects in our data we take in all types of legislation that a state might produce.  Generally looking at the bill_id will help you determine the type of legislation, but to make things easier across states we provide a `type` field on bills.  This field is a list with one (or more) of the following values:

Common Values:
* bill
* resolution
* joint resolution
* concurrent resolution
* constitutional amendment

Some states also make use of additional types such as 'contract', 'nomination', 'memorial' and more.

.. _action-categorization:

Action Types
------------

Although most states follow very similar parlimentary procedure the names that their bill status systems use
for various actions almost never match up.  To make analysis and the building of certain types of tools easier we attempt to categorize about 30 types of common actions.  In using our data you'll find these values in the `type` field of actions.

* **bill:introduced** - Bill is introduced or prefiled
* **bill:passed** - Bill has passed a chamber
* **bill:failed** - Bill has failed to pass a chamber
* **bill:withdrawn** - Bill has been withdrawn from consideration
* **bill:veto_override:passed** - The chamber attempted a veto override and succeeded
* **bill:veto_override:failed** - The chamber attempted a veto override and failed
* **bill:reading:1** - A bill has undergone its first reading
* **bill:reading:2** - A bill has undergone its second reading
* **bill:reading:3** - A bill has undergone its third (or final) reading
* **bill:filed** - A bill has been filed (for states where this is a separate event from bill:introduced)
* **bill:substituted** - A bill has been replaced with a substituted wholesale (called hoghousing in some states)
* **governor:received** - The bill has been transmitted to the governor for consideration
* **governor:signed** - The bill has signed into law by the governor
* **governor:vetoed** - The bill has been vetoed by the governor
* **governor:vetoed:line-item** - The governor has issued a line-item (partial) veto
* **amendment:introduced** - An amendment has been offered on the bill
* **amendment:passed** - The bill has been amended
* **amendment:failed** - An offered amendment has failed
* **amendment:amended** - An offered amendment has been amended (seen in Texas)
* **amendment:withdrawn** - An offered amendment has been withdrawn
* **amendment:tabled** - An amendment has been 'laid on the table' (generally preventing further consideration)
* **committee:referred** - The bill has been referred to a committee
* **committee:passed** - The bill has been passed out of a committee
* **committee:passed:favorable** - The bill has been passed out of a committee with a favorable report
* **committee:passed:unfavorable** - The bill has been passed out of a committee with an unfavorable report
* **committee:failed** - The bill has failed to make it out of committee
* **other** - All other actions will have a type of "other"

Vote Types
----------

Similarly to actions, we make an effort to categorize the motion being voted upon.  You'll find these values in the `type` field of vote objects.

Possible values:
* **passage** - This is a vote to pass (either out of committee or a chamber)
* **amendment** -  Vote on amending a bill
* **veto_override** - Vote to override an executive veto
* **reading:1** - Vote on a first reading
* **reading:2** - Vote on a second reading
* **other** - All other votes

.. _subject-categorization: 

Subjects
--------

Many states provide a list of subject areas for individual pieces of legislation.  We've made an attempt to map these to a comprehensive set of subjects.

If you're using the API data you'll find these in the `subjects` field if we've been able to categorize a state's bills.  If you're interested in the state's native categories those can be found in `scraped_subjects` field.

* Agriculture and Food
* Animal Rights and Wildlife Issues
* Arts and Humanities
* Budget, Spending, and Taxes
* Business and Consumers
* Campaign Finance and Election Issues
* Civil Liberties and Civil Rights
* Commerce
* Crime
* Drugs
* Education
* Energy
* Environmental
* Executive Branch
* Family and Children Issues
* Federal, State, and Local Relations
* Gambling and Gaming
* Government Reform
* Guns
* Health
* Housing and Property
* Immigration
* Indigenous Peoples
* Insurance
* Judiciary
* Labor and Employment
* Legal Issues
* Legislative Affairs
* Military
* Municipal and County Issues
* Nominations
* Other
* Public Services
* Recreation
* Reproductive Issues
* Resolutions
* Science and Medical Research
* Senior Issues
* Sexual Orientation and Gender Issues
* Social Issues
* State Agencies
* Technology and Communication
* Trade
* Transportation
* Welfare and Poverty
