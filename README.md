# Wayless
A lightweight object mapper...less should be more

The idea is to write less mapping code. Using Wayless you can take Object A and request an instance of Object B with field values mapped from A. By default fields are mapped by name; you can set to ignore casing. You're not limited to receiving a new instance, you can also pass in instances and map fields on existing objects. It's really intedned to help you stop writing tedious mapping code so you can concentrate on the bigger picture.

Mappings can be extended by calling the FieldMap and FieldSet methods.  

FieldMap expicitely created a relationship betweeen A.SomeProperty and B.SomeProperty. The idea behind FieldMap is to relate fields that don't share the same name and wouldn't have been linked by default.

    var mapper = new WaylessMap<SourceObject, DestinationObject>()
                        .FieldMap(d => d.AssignmentDate, s => s.TimeStamp);

FieldSet allows you to explicitly assign values to a property on the destination object.

    var mapper = new WaylessMap<SourceObject, DestinationObject>()
                        .FieldSet(d => d.CorrelationId, Guid.NewGuid());
                    
Both FieldMap and FieldSet use TypeConverter to try and assign values if types don't match.  

Using FieldSkip you can skip value assignment on the destination object.  A call to this method overrides both FieldMap and FieldSet.

    var mapper = new WaylessMap<SourceObject, DestinationObject>()
                        .FieldSkip(d => d.CorrelationId);
                    
Think of FieldSet, FieldMap, FieldSkip as rule creations.  None of these rules are applied until Map() called.

    // evalute rules and create instance of new object
    var destination = mapper.Map(TestSource);
