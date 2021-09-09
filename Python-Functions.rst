Inline Function Usage (python) 
------------------------------

Python Rules:
+++++++++++++

1. It must declear before the _columns

2. Method start with _ (underscore)

3. Passing the openERP parameters:
	self, cr, uid, ids, field_name, args, context

4. Return the object with object primary key (:id)

5. Return type is different from onchange function::
	result["unique key"] = DATA

.. code-block:: python

	def _check_float(self, cr, uid, ids, field_name, args, context=None):
		res = {}
		for floatData in self.browse(cr, uid, ids, context=context):
			if floatData.float_col >= 50:
				res[floatData.id] =  "yes, the float field it is much bigger than 50"
			else:
				res[floatData.id] =  "no, smaller than 50"
		return res
	#xml calling object
	<field name="float_col" string="float" on_change="
                        onchange_float(float_col)" />


_columns field rules:
+++++++++++++++++++++

1. Declear after inline functions

2. call inline function with fields.function(_MYFUNCTION, *optional parameters* )

3. When calling the inline function(_functions), return type needs to declear
   Otherwise, an error will raise

.. code-blocl:: python 
	'float2_col': fields.function(_double_float,type="float",store=False),



** Optional Parameters:** ::

    fnct, arg=None, fnct_inv=None, fnct_inv_arg=None, type="float",
        fnct_search=None, obj=None, method=False, store=False, multi=False



where

    :fnct: is the function or method that will compute the field 
      value. It must have been declared before declaring the functional field.
    :fnct_inv: is the function or method that will allow writing
      values in that field.
    :type: is the field type name returned by the function. It can
      be any field type name except function.
    :fnct_search: allows you to define the searching behaviour on
      that field.
    :method: whether the field is computed by a method (of an
      object) or a global function
    :store: If you want to store field in database or not. Default
      is False.
    :multi: is a group name. All fields with the same `multi`
      parameter will be calculated in a single function call. 





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
	If **multi** =True, must pass field_names as parameters
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