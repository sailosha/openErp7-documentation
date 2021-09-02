Graph views
-----------

A graph is a new mode of view for all views of type form. If, for example, a sale order line must be visible as list or as graph, define it like this in the action that open this sale order line. Do not set the view mode as "tree,form,graph" or "form,graph" - it must be "graph,tree" to show the graph first or "tree,graph" to show the list first. (This view mode is extra to your "form,tree" view and should have a separate menu item):

.. code-block:: xml

	 <field name="view_type">form</field>
	 <field name="view_mode">tree,graph</field>

view_type::

        tree = (tree with shortcuts at the left), form = (switchable view form/list) 

view_mode::

        tree,graph : sequences of the views when switching 

Then, the user will be able to switch from one view to the other. Unlike forms and trees, OpenERP is not able to automatically create a view on demand for the graph type. So, you must define a view for this graph:


.. code-block:: xml

	<record model="ir.ui.view" id="view_order_line_graph">
	   <field name="name">sale.order.line.graph</field>
	   <field name="model">sale.order.line</field>
	   <field name="type">graph</field>
	   <field name="arch" type="xml">
		 <graph string="Sales Order Lines">
		      <field name="product_id" group="True"/>
		      <field name="price_unit" operator="*"/>
		</graph>
	    </field>
	</record>


The graph view

A view of type graph is just a list of fields for the graph.

Graph tag
++++++++++

The default type of the graph is a pie chart - to change it to a barchart change **<graph string="Sales Order Lines">** to **<graph string="Sales Order Lines" type="bar">** You also may change the orientation.

:Example : 

.. code-block:: xml

	<graph string="Sales Order Lines" orientation="horizontal" type="bar">

Field tag
+++++++++

The first field is the X axis. The second one is the Y axis and the optional third one is the Z axis for 3 dimensional graphs. You can apply a few attributes to each field/axis:

    * **group**: if set to true, the client will group all item of the same value for this field. For each other field, it will apply an operator
    * **operator**: the operator to apply is another field is grouped. By default it's '+'. Allowed values are:

          + +: addition
          + \*: multiply
          + \**: exponent
          + min: minimum of the list
          + max: maximum of the list 

:Defining real statistics on objects:

The easiest method to compute real statistics on objects is:

   1. Define a statistic object which is a postgresql view
   2. Create a tree view and a graph view on this object 

You can get en example in all modules of the form: report\_.... Example: report_crm. 


Controlling view actions
------------------------

When defining a view, the following attributes can be added on the
opening element of the view (i.e. ``<form>``, ``<tree>``...)

``create``
        set to ``false`` to hide the link / button which allows to create a new
        record.

``delete``
        set to ``false`` to hide the link / button which allows to remove a
        record.

``edit``
        set to ``false`` to hide the link / button which allows to
        edit a record. 


These attributes are available on form, tree, kanban and gantt
views. They are normally automatically set from the access rights of
the users, but can be forced globally in the view definition. A
possible use case for these attributes is to define an inner tree view
for a one2many relation inside a form view, in which the user cannot
add or remove related records, but only edit the existing ones (which
are presumably created through another way, such as a wizard). 


Calendar Views
--------------

Calendar view provides timeline/schedule view for the data.

View Specification
++++++++++++++++++

Here is an example view:

.. code-block:: xml

    <calendar color="user_id" date_delay="planned_hours" date_start="date_start" string="Tasks">
        <field name="name"/>
        <field name="project_id"/>
    </calendar>

Here is the list of supported attributes for ``calendar`` tag:

    ``string``
        The title string for the view.

    ``date_start``
        A ``datetime`` field to specify the starting date for the calendar item. This 
        attribute is required.
        
    ``date_stop``
        A ``datetime`` field to specify the end date. Ignored if ``date_delay`` 
        attribute is specified.
        
    ``date_delay``
        A ``numeric`` field to specify time in hours for a record. This attribute
        will get preference over ``date_stop`` and ``date_stop`` will be ignored.
        
    ``day_length``
        An ``integer`` value to specify working day length. Default is ``8`` hours.
        
    ``color``
        A field, generally ``many2one``, to colorize calendar/gantt items.
        
    ``mode``
        A string value to set default view/zoom mode. For ``calendar`` view, this can be
        one of following (default is ``month``):
        
        * ``day``
        * ``week``
        * ``month``
   