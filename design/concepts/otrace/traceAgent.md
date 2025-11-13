concept traceAgent [Rules]
purpose represents an algorithm that checks sets of attestations against a set of rules
state 
rules: TraceAgent → set Rule
actions
eval(a: set Attestations) → i: set Issues
	a is evaluated against rules, returning a set of issues (empty if none)
addRule(r: Rule)
	r is added to rules
operational principle
	eval(a) evaluates a against rules and returns any issues, to be shown in the traceability 
service client according to user settings.

<!-- Rule 1: Use purposes
Explanation
For all data uses based on consents, the data type and purpose must match those in the consent. This is the rule violated in Use Case 1 (LendingClub loan)
Pseudocode
given DataReceipt
pass = False
for all dataUses in DataReceipt
     if basis is a consent
          for all terms in Consent
               if term.data == dataUse.data and term.purpose == dataUse.operation
                    pass = True
               end if
          end for
     end if
end for -->
