concept term [Data, Purpose, PurposeOntology]
purpose to define a clear and specific purpose for which a specific set of data could be used.
state
	// fields
	data: Term → Data
	purpose: Term → Purpose
	purposeOntology: Term → PurposeOntology
actions
define(d: Data, p: Purpose, po: PurposeOntology) 
	defines a potential agreement to use d for p as defined by po.
operational principle
	after define(d,p,po), the defined term can be referenced in consent-related attestations.