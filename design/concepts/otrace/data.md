concept data [DataType, DataTypeOntology, Provider, Subject, (Start, End)]
purpose to identify a specific dataset
state
	// fields
	dataType: Data → DataType
	dataTypeOntology: Data → DataTypeOntology
	controller: Data → Controller
	subject: Data → Subject
	(optional for timeseries) start: Data → Start
	(optional for timeseries) end: Data → End
actions
define(dt: DataType, dto: DataTypeOntology, c: Controller, s: Subject [st: Start, e: End]) 
	defines a dataset, with data of type dt as defined by dto, collected by c 
concerning s
operational principle
	after define(dt, dto, c, s), the defined dataset can be referenced in the future.