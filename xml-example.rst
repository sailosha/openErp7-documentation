Code Structure
---------------

The basic xml structure

.. code-block:: xml

    <?xml version="1.0"?>
    <openerp>
       <data>
           [view definitions]
       </data>
    </openerp>

The view definitions contain mainly three types of tags:

    * **<record>** tags with the attribute model="ir.ui.view", which contain the view definitions themselves
    * **<record>** tags with the attribute model="ir.actions.act_window", which link actions to these views
    * **<menuitem>** tags, which create entries in the menu, and link them with actions

An example for the form

.. code-block:: xml

    <record model="ir.ui.view" id="v">
        <field name="name">sale.order.form</field>
        <field name="model">sale.order</field>
        <field name="priority" eval="2"/>
        <field name="arch" type="xml">
	        <form string="Sale Order">
	            .........
	        </form>
        </field>
    </record>
