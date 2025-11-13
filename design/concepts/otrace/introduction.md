concept introduction [Controller, Subject, TraceService]
purpose to introduce a controller to the subject’s traceability service.
// fields
subject: Introduction → Subject
controller: Introduction → Controller
traceService: Introduction → TraceService
actions
	// called by subject
introduce(s: Subject, c: Controller, t: TraceService) → i: Introduction
	creates fresh introduction i such that
		i.subject = s
		i.controller = c
		i.traceService = t
	this represents a request by s for c to use t to trace their data interactions with s
// called by controller
acknowledge(c: Controller, s: Subject, j: introduction) 
	c agrees to trace their data interactions with s according to j
operational principle
	after introduce(s,c,t) and acknowledge (c,s,t), c and s must both use t to trace their data interactions