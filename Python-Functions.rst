Inline Function Usage (python) 
------------------------------

Rules:
++++++

1. It must declear before the _columns

2. Method start with _

3. Passing the openERP parameters::
	self, cr, uid, ids, field_name, args, context

4. Return the object with object primary key (AKA id)

.. code-block:: python

	def _check_float(self, cr, uid, ids, field_name, args, context=None):
		res = {}
		for floatData in self.browse(cr, uid, ids, context=context):
			# print floatData.id
			# print floatData.float_col
			if floatData.float_col >= 50:
				res[floatData.id] =  "yes, the float field it is much bigger than 50"
			else:
				res[floatData.id] =  "no, smaller than 50"
		return res	
