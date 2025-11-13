concept dataUse [Controller, Data, Basis, Subject]
purpose to use data about a subject for a specific purpose in association with a [legal] basis
state
	// fields
	controller: DataUse → Controller
	subject: DataUse → Subject
	data: DataUse → Data
	operation: DataUse → Operation
	operationOntology: DataUse → OperationOntology
	basis: DataUse → Basis
actions
// called by controller
use(c: Controller, s: Subject, d: Data, o: Operation, b: Basis) → u: DataUse
	creates fresh data use u such that
	u.controller = c
	u.subject = s
	u.data = d
	u.operation = o
	u.basis = b
// called by subject
getBasis(u: DataUse) → u.basis: Basis
operational principle
	Pair use and basis
