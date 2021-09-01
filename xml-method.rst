Here's the header of the initialization method of the class any field defined
in OpenERP inherits (as you can see in server/bin/osv/fields.py)::

    def __init__(self, string='unknown', required=False, readonly=False,
                 domain=None, context="", states=None, priority=0, change_default=False, size=None,
                 ondelete="set null", translate=False, select=False, **args) :

There are a common set of optional parameters that are available to most field
types:

:change_default: 
	Whether or not the user can define default values on other fields depending 
	on the value of this field. Those default values need to be defined in
	the ir.values table.
:help: 
	A description of how the field should be used: longer and more descriptive
	than `string`. It will appear in a tooltip when the mouse hovers over the 
	field.
:ondelete: 
	How to handle deletions in a related record. Allowable values are: 
	'restrict', 'no action', 'cascade', 'set null', and 'set default'.
:priority: Not used?
:readonly: `True` if the user cannot edit this field, otherwise `False`.
:required:
	`True` if this field must have a value before the object can be saved, 
	otherwise `False`.
:size: The size of the field in the database: number characters or digits.
:states:
	Lets you override other parameters for specific states of this object. 
	Accepts a dictionary with the state names as keys and a list of name/value 
	tuples as the values. For example: `states={'posted':[('readonly',True)]}`
:string: 
	The field name as it should appear in a label or column header. Strings
	containing non-ASCII characters must use python unicode objects. 
	For example: `'tested': fields.boolean(u'Testé')` 
:translate:
	`True` if the *content* of this field should be translated, otherwise 
	`False`.

There are also some optional parameters that are specific to some field types:

:context: 
	Define a variable's value visible in the view's context or an on-change 
	function. Used when searching child table of `one2many` relationship?
:domain: 
    Domain restriction on a relational field.

    Default value: []. 

    Example: domain=[('field','=',value)])
:invisible: Hide the field's value in forms. For example, a password.
:on_change:
	Default value for the `on_change` attribute in the view. This will launch
	a function on the server when the field changes in the client. For example,
	`on_change="onchange_shop_id(shop_id)"`. 
:relation:
	Used when a field is an id reference to another table. This is the name of
	the table to look in. Most commonly used with related and function field
	types.
:select: 
	Default value for the `select` attribute in the view. 1 means basic search,
	and 2 means advanced search.

Type of Fields
--------------

Basic Types
+++++++++++

:boolean:

	A boolean (true, false).

	Syntax::

                fields.boolean('Field Name' [, Optional Parameters]),

:integer:

	An integer.

	Syntax::

                fields.integer('Field Name' [, Optional Parameters]),

:float:

    A floating point number.

    Syntax::

                fields.float('Field Name' [, Optional Parameters]),

    .. note::

            The optional parameter digits defines the precision and scale of the
            number. The scale being the number of digits after the decimal point
            whereas the precision is the total number of significant digits in the
            number (before and after the decimal point). If the parameter digits is
            not present, the number will be a double precision floating point number.
            Warning: these floating-point numbers are inexact (not any value can be
            converted to its binary representation) and this can lead to rounding
            errors. You should always use the digits parameter for monetary amounts.

    Example::

        'rate': fields.float(
            'Relative Change rate',
            digits=(12,6) [,
            Optional Parameters]),

:char:

  A string of limited length. The required size parameter determines its size.

  Syntax::

  	fields.char(
  		'Field Name', 
  		size=n [, 
  		Optional Parameters]), # where ''n'' is an integer.

  Example::

        'city' : fields.char('City Name', size=30, required=True),

:text:

  A text field with no limit in length.

  Syntax::

                fields.text('Field Name' [, Optional Parameters]),

:date:

  A date.

  Syntax::

                fields.date('Field Name' [, Optional Parameters]),

:datetime:

  Allows to store a date and the time of day in the same field.

  Syntax::

                fields.datetime('Field Name' [, Optional Parameters]),

:binary:

  A binary chain

:selection:

  A field which allows the user to make a selection between various predefined values.

  Syntax::

                fields.selection((('n','Unconfirmed'), ('c','Confirmed')),
                                   'Field Name' [, Optional Parameters]),

  .. note::

             Format of the selection parameter: tuple of tuples of strings of the form::

                (('key_or_value', 'string_to_display'), ... )
                
  .. note::
             You can specify a function that will return the tuple. Example ::
             
                 def _get_selection(self, cursor, user_id, context=None):
                     return (
                     	('choice1', 'This is the choice 1'), 
                     	('choice2', 'This is the choice 2'))
                     
                 _columns = {
                    'sel' : fields.selection(
                    	_get_selection, 
                    	'What do you want ?')
                 }

  *Example*

  Using relation fields **many2one** with **selection**. In fields definitions add::

        ...,
        'my_field': fields.many2one(
        	'mymodule.relation.model', 
        	'Title', 
        	selection=_sel_func),
        ...,

  And then define the _sel_func like this (but before the fields definitions)::

        def _sel_func(self, cr, uid, context=None):
            obj = self.pool.get('mymodule.relation.model')
            ids = obj.search(cr, uid, [])
            res = obj.read(cr, uid, ids, ['name', 'id'], context)
            res = [(r['id'], r['name']) for r in res]
            return res

Relational Types
++++++++++++++++

:one2one:

  A one2one field expresses a one:to:one relation between two objects. It is
  deprecated. Use many2one instead.

  Syntax::

                fields.one2one('other.object.name', 'Field Name')

:many2one:

  Associates this object to a parent object via this Field. For example
  Department an Employee belongs to would Many to one. i.e Many employees will
  belong to a Department

  Syntax::

		fields.many2one(
			'other.object.name', 
			'Field Name', 
			optional parameters)

  Optional parameters:
  
    - ondelete: What should happen when the resource this field points to is deleted.
            + Predefined value: "cascade", "set null", "restrict", "no action", "set default"
            + Default value: "set null"
    - required: True
    - readonly: True
    - select: True - (creates an index on the Foreign Key field)

  *Example* ::

                'commercial': fields.many2one(
                	'res.users', 
                	'Commercial', 
                	ondelete='cascade'),

:one2many:

  TODO

  Syntax::

                fields.one2many(
                	'other.object.name', 
                	'Field relation id', 
                	'Fieldname', 
                	optional parameter)

  Optional parameters:
                - invisible: True/False
                - states: ?
                - readonly: True/False

  *Example* ::

                'address': fields.one2many(
                	'res.partner.address', 
                	'partner_id', 
                	'Contacts'),

:many2many:

        TODO

        Syntax::

                fields.many2many('other.object.name',
                                 'relation object',
                                 'actual.object.id',
                                 'other.object.id',                                 
                                 'Field Name')

        Where:
                - other.object.name is the other object which belongs to the relation
                - relation object is the table that makes the link
                - actual.object.id and other.object.id are the fields' names used in the relation table

        Example::

                'category_ids':
                   fields.many2many(
                    'res.partner.category',
                    'res_partner_category_rel',
                    'partner_id',
                    'category_id',
                    'Categories'),

        To make it bidirectional (= create a field in the other object)::

                class other_object_name2(osv.osv):
                    _inherit = 'other.object.name'
                    _columns = {
                        'other_fields': fields.many2many(
                            'actual.object.name', 
                            'relation object', 
                            'actual.object.id', 
                            'other.object.id', 
                            'Other Field Name'),
                    }
                other_object_name2()

        Example::

                class res_partner_category2(osv.osv):
                    _inherit = 'res.partner.category'
                    _columns = {
                        'partner_ids': fields.many2many(
                            'res.partner', 
                            'res_partner_category_rel', 
                            'category_id', 
                            'partner_id', 
                            'Partners'),
                    }
                res_partner_category2()

:related:

  Sometimes you need to refer to the relation of a relation. For example,
  supposing you have objects: City -> State -> Country, and you need to refer to
  the Country from a City, you can define a field as below in the City object::

        'country_id': fields.related(
            'state_id', 
            'country_id', 
            type="many2one",
            relation="res.country",
            string="Country", 
            store=False)

  Where:
  	- The first set of parameters are the chain of reference fields to
  	  follow, with the desired field at the end.
  	- :guilabel:`type` is the type of that desired field.
  	- Use :guilabel:`relation` if the desired field is still some kind of
  	  reference. :guilabel:`relation` is the table to look up that
  	  reference in.


