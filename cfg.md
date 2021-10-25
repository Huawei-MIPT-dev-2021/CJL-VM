# Context Free Grammar
class_file
		: class **id** optional_inheritance **{** class_body **}**

optional_inheritance
		: **inherits** **id**
		| e

class_body
		: public_part private_part protected_part

public_part
		: **public** **:** fields_and_methods
		| e

private_part
		: **private** **:** fields_and_methods
		| e

protected_part
		: **protected** **:** fields_and_methods
		| e

fields_and_methods
		: field fields_and_methods
		| method fields_and_methods
		| e

field
		: optional_const **type** **id** **;**
		| static optional_const **type** **id** **=** expr **;**

optional_const
		: **const**
		| e

method
		: **id** **(** args_list **)** optional_static optional_const optional_override **->** **type** **{** method_body **}**

optional_static
		: **static**
		| e

optional_override
		: **override**
		| e

args_list
		: **type** **id** args_list_reminder
		| e

args_list_reminder
		: **,** **type** **id** args_list_reminder
		| e

method_body
		: if_clause method_body
		| for_loop method_body
		| while_loop method_body
		| function_call method_body
		| var_def method_body
		| return_statement method_body
		| break_or_continue method_body
		| e

break_or_continue
		: **break** **;**
		| **continue** **;**

return_statement
		: **return** expr **;**
		| **return** **;**

var_def
		: optional_type **id** **;**
		| optional_type **id** **=** expr **;**
		| optional_type **id** **=** function_call **;**

optional_type
		: **type**
		| e

function_call
		: **id** **(** call_args_list **)** **;**

call_args_list
		: **id** call_args_list_reminder
		| e

call_args_list_reminder
		: **,** **id** call_args_list_reminder
		| e

<!-- while_loop
		: **while** predicat **{** method_body **}**

predicat
		: predicat1 predicat2

lpredicat
		:  -->
