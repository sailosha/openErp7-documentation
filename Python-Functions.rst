Inline Function Usage (python) 
------------------------------

Python Rules:
+++++++++++++

1. It must declear before the _columns

2. Method start with _ 

3. Passing the openERP parameters
	self, cr, uid, ids, field_name, args, context

4. Return the object with object primary key (:id)

5. 


_columns field rules:
+++++++++++++++++++++

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

5. field_name OR field_names ?

:field_name: 
	Passing the single column name at one time.

:field_names:
	If **multi**=True, must pass field_names as parameters
	Passing all the column names together.


















Another example for using SQL query in the function

.. code-block:: python

	def _get_cur_function_id(self, cr, uid, ids, field_name, arg, context):
	    for i in ids:
	        #get the id of the current function of the employee of identifier "i"
	        sql_req= """
	        SELECT f.id AS func_id
	        FROM hr_contract c
	          LEFT JOIN res_partner_function f ON (f.id = c.function)
	        WHERE
	          (c.employee_id = %d)
	        """ % (i,)

	        cr.execute(sql_req)
	        sql_res = cr.dictfetchone()

	        if sql_res: #The employee has one associated contract
	            res[i] = sql_res['func_id']
	        else:
	            #res[i] must be set to False and not to None because of XML:RPC
	            # "cannot marshal None unless allow_none is enabled"
	            res[i] = False
	    return res