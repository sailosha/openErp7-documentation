Fcuntion, atrributes Usage in XML 
---------------------------------

1. When using attrs in field. Make sure follow the synatax ::

	attrs={'ATTRIBUTE': [***(arguements)...]}

.. code-block:: xml
 	<field name="invisable_view" string="this is min_quantity for this product" attrs="{'invisible':[('default_code','!=','06')]}"


 2. When using invisible for the field, you should create another table to read the data from this field.:: 

 	field1 : get data from python, not use in the form
 	field2 : read data from field1, decide to show or not 