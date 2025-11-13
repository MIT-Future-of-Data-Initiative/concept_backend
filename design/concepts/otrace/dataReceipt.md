concept dataReceipt [Controller, DataUses, Subject]
purpose to proclaim a set of dataUses as a complete action or decision
state
	// fields
	controller: DataReceipt → Subject
	subject: DataReceipt → Subject
	dataUses: DataReceipt → set DataUse
actions
// called by controller
create(c: Controller, s: Subject, dus: DataUses) → r: DataReceipt
	creates fresh dataReceipt r such that
	r.controller = c
	r.subject = s
	r.dataUses = dus
operational principle
	Controllers create data receipts for a user to view through a traceService.
