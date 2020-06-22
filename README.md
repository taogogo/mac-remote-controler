# mac-remote-controler
control your mac remotely, free, open source, simple

#install the script on your mac

1, switch to superuser: sudo su -
2, create script and write down the code
remotecontroit.php
```
<?php
//////////////////
///////////for($i=0; $i<55;$i++){
$cmd = file_get_contents('http://your-server.com/controlit.php?device_no=001');
exec($cmd, $exeresult);
var_dump($exeresult);
///////////sleep(1);
////////////}



/////////////////////
//sleep(60);
/*
$response = null;
system("/sbin/ping -c 1 google.vn", $response);
if($response == 0)
{
	var_dump('network ok');
} else {
	var_dump('not ok, try again');
	sleep(300);

	$response = null;
	system("/sbin/ping -c 1 google.vn", $response);
	if($response == 0)
	{
	} else {
		echo "retry but still not ok";
		exec("/sbin/reboot");
		#exec("/sbin/shutdown  -h +10");

	}



}
*/


```

#set crontab

```
* * * * * /usr/bin/php /var/root/remotecontrolit.php

```

#control it

controlit.php

```

<?php
$device_no = $_REQUEST['device_no'] ? $_REQUEST['device_no'] : '';
$show_result = $_REQUEST['only_show_result'];
$cmd_str = $_REQUEST['cmd'];
$txt = file_get_contents('cmdlist'.$device_no.'.txt');
if($show_result){
//var_dump('show result:', $txt);
if(!empty($cmd_str)){

	file_put_contents('cmdlist'.$device_no.'.txt',$cmd_str);
	echo "runit";
}
$checked_str[$device_no] = 'selected';
echo <<<EOT
<a href="http://your-server.com/controlit.php?only_show_result=1&device_no=">Windows</a>
<a href="http://your-server.com/controlit.php?only_show_result=1&device_no=001">Macbook Pro</a>
<hr />
<form action="" method="post">
<input name="cmd" /><button>run</button>
<select name="device_no">
  <option value ="" {$checked_str['']}>Windows-test</option>
  <option value ="001" {$checked_str['001']}>Macbook Pro OLD</option>
  <option value ="002" {$checked_str['002']}>Macbook Pro NEW</option>
</select>
</form>
EOT;


echo "<pre>";
echo `tail cmdlist_his{$device_no}.log`;
echo "</pre>";


}
else{
echo $txt;
if(!empty($txt)){
	file_put_contents('cmdlist'.$device_no.'.txt','');
}

file_put_contents('cmdlist_his'.$device_no.'.log',date("Y-m-d H:i:s").":{$txt}\n",FILE_APPEND);


}

```
