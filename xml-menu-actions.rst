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

An example to show the menu inherit in the XML

.. code-block:: xml
        

        <menuitem name="Table" id="menu_base_new_table" sequence="1"  />
        <menuitem name="New Header" id="menu_new_table_header"  parent="menu_base_new_table"  sequence="10"/>
        <menuitem name="New Table" id="menu_new_table"  parent="menu_new_table_header" action="action_new_table" sequence="10"/>

.. image::  https://github.com/sailosha/openErp7-documentation/blob/main/Why%20not%20openERP7/img01.png?raw=true


Actions
=======

openERP must use "Actions" inorder to change on click, search and so on.

Actions are explained in more detail in section "Administration Modules - Actions". Here's the template of an action XML record :
::

    <record model="ir.actions.act_window" id="action_id_1">
        <field name="name">action.name</field>
        <field name="view_id" ref="view_id_1"/>
        <field name="domain">["list of 3-tuples (max 250 characters)"]</field>
        <field name="context">{"context dictionary (max 250 characters)"}</field>
        <field name="res_model">Open.object</field>
        <field name="view_type">form|tree</field>
        <field name="view_mode">form,tree|tree,form|form|tree</field>
        <field name="usage">menu</field>
        <field name="target">new</field>
    </record>

**Where**

    * **id** is the identifier of the action in the table "ir.actions.act_window". It must be unique.
    * **name** is the name of the action (mandatory).
    * **view_id** is the name of the view to display when the action is activated. If this field is not defined, the view of a kind (list or form) associated to the object res_model with the highest priority field is used (if two views have the same priority, the first defined view of a kind is used).
    * **domain** is a list of constraints used to refine the results of a selection, and hence to get less records displayed in the view. Constraints of the list are linked together with an AND clause : a record of the table will be displayed in the view only if all the constraints are satisfied.
    * **context** is the context dictionary which will be visible in the view that will be opened when the action is activated. Context dictionaries are declared with the same syntax as Python dictionaries in the XML file. For more information about context dictionaries, see section " The context Dictionary".
    * **res_model** is the name of the object on which the action operates.
    * **view_type** is set to form when the action must open a new form view, and is set to tree when the action must open a new tree view.
    * **view_mode** is only considered if view_type is form, and ignored otherwise. The four possibilities are :
          - **form,tree** : the view is first displayed as a form, the list view can be displayed by clicking the "alternate view button" ;
          - **tree,form** : the view is first displayed as a list, the form view can be displayed by clicking the "alternate view button" ;
          - **form** : the view is displayed as a form and there is no way to switch to list view ;
          - **tree** : the view is displayed as a list and there is no way to switch to form view.

    * **target** the view will open in new window like wizard.
    * **context** will be passed to the action itself and added to its global context

      .. code-block:: xml

          <record model="ir.actions.act_window" id="a">
              <field name="name">account.account.tree1</field> 
              <field name="res_model">account.account</field> 
              <field name="view_type">tree</field> 
              <field name="view_mode">form,tree</field> 
              <field name="view_id" ref="v"/> 
              <field name="domain">[('code','=','0')]</field> 
              <field name="context">{'project_id': active_id}</field> 
          </record>



They indicate at the user that he has to open a new window in a new 'tab'.

Administration > Custom > Low Level > Base > Action > Window Actions

