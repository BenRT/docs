# Session

- [Configuration](#configuration)
- [Session Usage](#session-usage)
- [Flash Data](#flash-data)
- [Database Sessions](#database-sessions)
- [Session Drivers](#session-drivers)

<a name="configuration"></a>
## Configuration

Since HTTP driven applications are stateless, sessions provide a way to store information about the user across requests. Laravel ships with a variety of session back-ends available for use through a clean, unified API. Support for popular back-ends such as [Memcached](http://memcached.org), [Redis](http://redis.io), and databases is included out of the box.

The session configuration is stored in `app/config/session.php`. Be sure to review the well documented options available to you in this file. By default, Laravel is configured to use the `native` session driver, which will work well for the majority of applications.

<a name="session-usage"></a>
## Session Usage

**Storing An Item In The Session**

	Session::put('key', 'value');

**Retrieving An Item From The Session**

	$value = Session::get('key');

**Retrieving An Item Or Returning A Default Value**

	$value = Session::get('key', 'default');

	$value = Session::get('key', function() { return 'default'; });

**Determining If An Item Exists In The Session**

	if (Session::has('users'))
	{
		//
	}

**Removing An Item From The Session**

	Session::forget('key');

**Removing All Items From The Session**

	Session::flush();

**Regenerating The Session ID**

	Session::regenerate();

<a name="flash-data"></a>
## Flash Data

Sometimes you may wish to store items in the session only for the next request. You may do so using the `Session::flash` method:

	Session::flash('key', 'value');

**Reflashing The Current Flash Data For Another Request**

	Session::reflash();

**Reflashing Only A Subset Of Flash Data**

	Session::keep(array('username', 'email'));

<a name="database-sessions"></a>
## Database Sessions

When using the `database` session driver, you will need to setup a table to contain the session items. Below is an example `Schema` declaration for the table:

	Schema::create('sessions', function($table)
	{
		$table->string('id')->unique();
		$table->text('payload');
		$table->integer('last_activity');
	});

Of course, you may use the `session:table` Artisan command to generate this migration for you!

	php artisan session:table

	composer dump-autoload

	php artisan migrate
	
<a name="session-drivers"></a>
## Session Drivers

The session "driver" defines where session data will be stored for each request. Laravel ships with several great drivers out of the box:

- `native` - sessions will be handled by internal PHP session facilities.
- `cookie` - sessions will be stored in secure, encrypted cookies.
- `database` - sessions will be stored in a database used by your application.
- `memcached` / `redis` - sessions will be stored in one of these fast, cached based stores.
- `array` - sessions will be stored in a simple PHP array and will not be persisted across requests.

> **Note:** The array driver is typically used for running [unit tests](/docs/testing), so no session data will be persisted.