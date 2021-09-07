=================
Menus and Actions 
=================

Menus
=====

Menus are records in the ``ir.ui.menu`` table. In order to create a new
menu entry, you can directly create a record using the ``record`` tag.

.. code-block:: xml

        <record id="menu_xml_id" model="ir.ui.menu">
          <field name="name">My Menu</field>
          <field name="action" ref="action_xml_id"/>
          <field name="sequence" eval="<integer>"/>
          <field name="parent_id" ref="parent_menu_xml_id"/>
        </record>

There is a shortcut by using the ``menuitem`` tag that **you should use
preferentially**. It offers a flexible way to easily define the menu entry
along with icons and other fields.

.. code-block:: xml

    <menuitem id="menu_xml_id" 
        name="My Menu" 
        action="action_xml_id" 
        icon="NAME_FROM_LIST" 
        groups="groupname" 
        sequence="<integer>"
    parent="parent_menu_xml_id"
    />

Where

 - ``id`` specifies the **xml identifier** of the menu item in the menu
   items table. This identifier must be unique. Mandatory field.
 - ``name`` defines the menu name that will be displayed in the client.
   Mandatory field.
 - ``action`` specifies the identifier of the attached action defined
   in the action table (``ir.actions.act_window``). This field is not
   mandatory : you can define menu elements without associating actions
   to them. This is useful when defining custom icons for menu elements
   that will act as folders. This is how custom icons for "Projects" or
   "Human Resources" in OpenERP are defined).
 - ``groups`` specifies which group of user can see the menu item.
   (example : groups="admin"). See section " Management of Access Rights"
   for more information. Multiple groups should be separated by a ','
   (example: groups="admin,user")
 - ``sequence`` is an integer that is used to sort the menu item in the
   menu. The higher the sequence number, the downer the menu item. This
   argument is not mandatory: if sequence is not specified, the menu item
   gets a default sequence number of 10. Menu items with the same sequence
   numbers are sorted by order of creation (``_order = "*sequence,id*"``).

The main current limitation of using ``menuitem`` is that the menu action must be an
``act_window`` action. This kind of actions is the most used action in OpenERP.
However for some menus you will use other actions. For example, the Feeds
page that comes with the mail module is a client action. For this kind of
menu entry, you can combine both declaration, as defined in the mail module :

.. code-block:: xml

        <!-- toplevel menu -->
        <menuitem id="mail_feeds_main" name="Feeds" sequence="0"
            web_icon="static/src/img/feeds.png"
            web_icon_hover="static/src/img/feeds-hover.png" />
        <record id="mail_feeds_main" model="ir.ui.menu">
            <field name="action" ref="action_mail_all_feeds"/>
        </record>

.. code-block:: xml
        

        <menuitem name="Table" id="menu_base_new_table" sequence="1"  />
        <menuitem name="New Header" id="menu_new_table_header"  parent="menu_base_new_table"  sequence="10"/>
        <menuitem name="New Table" id="menu_new_table"  parent="menu_new_table_header" action="action_new_table" sequence="10"/>

.. image::  https://github.com/sailosha/openErp7-documentation/blob/main/Why%20not%20openERP7/img01.png?raw=true
