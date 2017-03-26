                                                                ការណែនាំឱ្យស្គាល់
      Migrations ដូចនឹង version control របស់ databse  ដែលអនុញ្ញាត ឱ្យ ក្រុមរបស់ អ្នក ងាយស្រួលកែប្រែ និង ចែករំលែក database schmea  របស់
application ។ ប្រសិនបើអ្នកមិនចង់ ប្រាប់ មិត្តរួមការងារបស់អ្នក បន្ថែម column មួយទៅកាន់ local database schema របស់ពួកគេ ៕ អ្នកអាចប្រឈមនឹង បញ្ហារ
ដែល database migration solve ។

       Laravel Schema facade បានផ្តល់ជួន database agnostic ដែល support ដល់ការបង្កើត និង ធ្វើការកែប្រែផ្សេងៗ លើ tables across all of Laravel's supported database systems។


                                                          Generating Migrations
        ដើម្បីបង្កើត migration យើងប្រើ make:migration artisan command ដូចខាងក្រោម:

              php artisan make:migration create_users_table
              
       migration ថ្មីនឹង ត្រូវបាន ដាក់នៅក្នុ directory database/migrations ។ migration file នីមួយៗ ផ្ទុក timestamp ដែលអនុញ្ញាតឱ្យ laravel
កំណត់ លំដាប់ migrations ។ 
      --table និង --create options អាចត្រូវប្រើដើម្បី ដាក់ឈ្មោះ table ដែល Migration នឹងបង្កើត ក្នុង database  ។  
     These options simply pre-fill the generated migration stub file with the specified table:

            php artisan make:migration create_users_table --create=users

            php artisan make:migration add_votes_to_users_table --table=users
     
     ប្រសិនអ្នកចង់ កំណត់ path សំរាប់ បង្កើត migration ទៅទីតាំងនោះអ្នកត្រូវប្រើ --path option នៅពេល execute make:migration command។
path គួរតែ relative to application's base path ។ 


                                        Migration Structure
        migration class ផ្ទុកmethod ពីរ គឺ up និង down ។ up method ត្រូវបានប្រើ ដើម្បី បង្កើត table  columns ឬ indexes ទៅកាន់ database របស់អ្នក
នៅខណៈដែល down method ត្រូវបាន ផ្ទុយនឹង up method ខាងលើ . 

នៅក្នុង methods ទាំងពីរខាងលើ អាចប្រើ Laravel schema builder ដើម្បីបង្កើត និង កែប្រែ tables ។ ដើម្បីរៀន អំពី methods ទាំងអស់ អាចរកបាន នៅលើ
Schema builder  ដើម្បីឱ្យកាន់តែច្បាស់សូមមើល ឯកសាររបស់វា ។ នៅក្នុង ឩទាហរណ៍ ខាងក្រោម យើងបង្កើត flights table 

        <?php

        use Illuminate\Support\Facades\Schema;
        use Illuminate\Database\Schema\Blueprint;
        use Illuminate\Database\Migrations\Migration;

        class CreateFlightsTable extends Migration
        {
            /**
             * Run the migrations.
             *
             * @return void
             */
            public function up()
            {
                Schema::create('flights', function (Blueprint $table) {
                    $table->increments('id');
                    $table->string('name');
                    $table->string('airline');
                    $table->timestamps();
                });
            }

            /**
             * Reverse the migrations.
             *
             * @return void
             */
            public function down()
            {
                Schema::drop('flights');
            }
        }

                                                      Running Migrations

          ដើម្បី run migrations ទាំងអស់ សូម execute migrate Artisan command :
                  php artisan migrate
                  
         ប្រើសិនអ្នក កំពុងប្រើ homestead virtual machin អ្នកគួតែ run command ខាងក្រោម នៅក្នុង virtual machine។
         
        Forcing Migrations To Run In Production
        
       migration operations មួយចំនួន គឺជា destructive ដែលមានន័យថា វាអាចបណ្តាល ឱ្យបាត់បង់ data ។ នៅក្នុងគោលបំណងដើម្បី ការអ្នកកុំឱ្យ
run command ទាំងនេះទៅលើ production database អ្នកអាច prompt ដើម្បីបញ្ជាក់ ការប្រតិបត្តិ command ខាងលើ ។ ដើម្បី run commands ខាងលើនេះដោយមិន
ចាំបាច់ prompt អ្នកត្រូវប្រើ  --force flag:
        
        php artisan migrate --force

                                                Rolling Back Migrations

        ដើម្បី rollback latest migration operation អ្នកត្រូវប្រើ rollback command ។ command នេះ roll back "batch" ចំងក្រោយ នៃ migrations
ដែលអាចបន្ថែម multiple migration files :
                            
        php artisan migrate:rollback


        អ្នកអាច rollback ចំនួនកំណត់មួយរបស់ migrations ដោយ ប្រើ step option តាមរយៈ rollback command ។ ឩទាហរណ៍ command ខាងក្រោម នឹង 
rollback five migration ចុងក្រោយ :

          php artisan migrate:rollback --step=5
          
          
         migrate:reset command នឹង rollback រាល់  application's migrations ទាំងអស់ : 

            php artisan migrate:reset
                                
                                          Rollback & Migrate In Single Command
        migrate:refresh command roll back migrations ទាំងអស់ ហើយបន្ទាប់មក execute migrate command ។
command នេះត្រូវប្រើ ដើម្បីបង្កើត database ឡើងវិញទាំងមួល :
      
      php artisan migrate:refresh
      // Refresh the database and run all database seeds...     
      php artisan migrate:refresh --seed
      
      អ្នកអាច rollback និង re-migrate ចនួនកំណត់ នៃ migrations ដោយ step option តាមរយៈ refresh command 
command ខាងក្រោម នឹង  rollback និង បង្កើត migration ប្រាំចុងក្រោយ ឡើងវិញ :

      php artisan migrate:refresh --step=5

                                                  Tables


        Creating Tables
        ដើម្បីបង្កើត database table ថ្មីមួយ យើងប្រើ create method នៅលើ Schema facade ។ create method 
 ចាប់យក arguments ពីរ ដែល argument ទីមួយគឺជា ឈ្មោះ table ហើយ second គឺជា Closure ដែល ទទួល យក Blueprint object 
 ដែលអាចត្រូវប្រើដើម្បី define table ថ្មីមួយ :


        Schema::create('users', function (Blueprint $table) {
            $table->increments('id');
        });
        
        នៅពេល បង្កើត table អ្នកអាចប្រើ schema builder's column method ណាមួយ ដើម្បី define table's columns ។

                                Checking For Table / Column Existence
        ដើម្បីងាយស្រួលក្នុងការ ពិនិត្យ មើលថា table ឬក៏ column មាន ឬអត់ យើងប្រើ hasTable ឬ hasColumn methods :
     
            if (Schema::hasTable('users')) {
                //
            }

            if (Schema::hasColumn('users', 'email')) {
                //
            }
                                              
                                              Connection & Storage Engine
          ប្រសិនបើអ្នកចង់ perform schema operation នៅលើ database connection ដែលមិនមែនជា default connection
 អ្នកត្រូវប្រើ method connection ដូចខាងក្រោមៈ

            Schema::connection('foo')->create('users', function (Blueprint $table) {
                $table->increments('id');
            });
        
        
        អ្នកអាចប្រើ engine property នៅលើ schema bulder ដើម្បី define table's storage engine:
        
            Schema::create('users', function (Blueprint $table) {
                $table->engine = 'InnoDB';

                $table->increments('id');
            });
            

                    Renaming / Dropping Tables

        ដើម្បី rename existing database table យើងប្រើ rename method ដូចខាងក្រោម :
        
            Schema::rename($from, $to);
        ដើម្បីលុប table នៅក្នុង database អ្នកអាចប្រើ drop ឬ dropIfExists methods:
       
            Schema::drop('users');
            Schema::dropIfExists('users');
            
                                      Renaming Tables With Foreign Keys
        មុនពេល rename table អ្នកគួរតែ verify any foreign key constraints នៅលើ table ដោយ explicit name នៅក្នុង migration files 
ជំនួស  letting Laravel assign a convention based name. Otherwise, the foreign key constraint name will refer to the old table name.


                                        Columns


            Creating Columns
        table method នៅលើ Schema facade ដែលត្រូវបានប្រើសំរាប់ update existing tables ។ ដូចនឹង create metho 
table method accepts arguments ពីរ គឺ ឈ្មោះ table និង Closure ដែល receive Blueprint instance អ្នកអាចប្រើ
ដើម្បី បន្ថែម columns ទៅកាន់ table :

            Schema::table('users', function (Blueprint $table) {
                $table->string('email');
            });
          
          
            Available Column Types
        
        schema builder contains variety នៃ column types ដែលអ្នកអាច កំណត់ នៅពេល បង្កើត tables 

          Command	Description
          
            $table->bigIncrements('id');	Incrementing ID (primary key) using a "UNSIGNED BIG INTEGER" equivalent.
            $table->bigInteger('votes');	BIGINT equivalent for the database.
            $table->binary('data');	BLOB equivalent for the database.
            $table->boolean('confirmed');	BOOLEAN equivalent for the database.
            $table->char('name', 4);	CHAR equivalent with a length.
            $table->date('created_at');	DATE equivalent for the database.
            $table->dateTime('created_at');	DATETIME equivalent for the database.
            $table->dateTimeTz('created_at');	DATETIME (with timezone) equivalent for the database.
            $table->decimal('amount', 5, 2);	DECIMAL equivalent with a precision and scale.
            $table->double('column', 15, 8);	DOUBLE equivalent with precision, 15 digits in total and 8 after the decimal point.
            $table->enum('choices', ['foo', 'bar']);	ENUM equivalent for the database.
            $table->float('amount', 8, 2);	FLOAT equivalent for the database, 8 digits in total and 2 after the decimal point.
            $table->increments('id');	Incrementing ID (primary key) using a "UNSIGNED INTEGER" equivalent.
            $table->integer('votes');	INTEGER equivalent for the database.
            $table->ipAddress('visitor');	IP address equivalent for the database.
            $table->json('options');	JSON equivalent for the database.
            $table->jsonb('options');	JSONB equivalent for the database.
            $table->longText('description');	LONGTEXT equivalent for the database.
            $table->macAddress('device');	MAC address equivalent for the database.
            $table->mediumIncrements('id');	Incrementing ID (primary key) using a "UNSIGNED MEDIUM INTEGER" equivalent.
            $table->mediumInteger('numbers');	MEDIUMINT equivalent for the database.
            $table->mediumText('description');	MEDIUMTEXT equivalent for the database.
            $table->morphs('taggable');	Adds unsigned INTEGER taggable_id and STRING  taggable_type.
            $table->nullableMorphs('taggable');	Nullable versions of the morphs() columns.
            $table->nullableTimestamps();	Nullable versions of the timestamps() columns.
            $table->rememberToken();	Adds remember_token as VARCHAR(100) NULL.
            $table->smallIncrements('id');	Incrementing ID (primary key) using a "UNSIGNED SMALL INTEGER" equivalent.
            $table->smallInteger('votes');	SMALLINT equivalent for the database.
            $table->softDeletes();	Adds nullable deleted_at column for soft deletes.
            $table->string('email');	VARCHAR equivalent column.
            $table->string('name', 100);	VARCHAR equivalent with a length.
            $table->text('description');	TEXT equivalent for the database.
            $table->time('sunrise');	TIME equivalent for the database.
            $table->timeTz('sunrise');	TIME (with timezone) equivalent for the database.
            $table->tinyInteger('numbers');	TINYINT equivalent for the database.
            $table->timestamp('added_on');	TIMESTAMP equivalent for the database.
            $table->timestampTz('added_on');	TIMESTAMP (with timezone) equivalent for the database.
            $table->timestamps();	Adds nullable created_at and updated_at columns.
            $table->timestampsTz();	Adds nullable created_at and updated_at (with timezone) columns.
            $table->unsignedBigInteger('votes');	Unsigned BIGINT equivalent for the database.
            $table->unsignedInteger('votes');	Unsigned INT equivalent for the database.
            $table->unsignedMediumInteger('votes');	Unsigned MEDIUMINT equivalent for the database.
            $table->unsignedSmallInteger('votes');	Unsigned SMALLINT equivalent for the database.
            $table->unsignedTinyInteger('votes');	Unsigned TINYINT equivalent for the database.
            $table->uuid('id');	UUID equivalent for the database.

            Column Modifiers
        បន្ថែមលើ column types list ខាងលើ មាន column modifier ជាច្រើន ដែលអ្នកឱ្យអ្នកប្រើ ដើម្បី បន្ថែម column មួយទៅ database table ។
 អ្នកអាច បង្កើត column nullable ដោយប្រើ nullable method :

        Schema::table('users', function (Blueprint $table) {
            $table->string('email')->nullable();
        });
        
        list នៃ column modifiers ខាងក្រោម ទាំងអស់ ។ List នេះមិន មាន index modifiers ទេ:

        Modifier	Description
            ->after('column')	Place the column "after" another column (MySQL Only)
            ->comment('my comment')	Add a comment to a column
            ->default($value)	Specify a "default" value for the column
            ->first()	Place the column "first" in the table (MySQL Only)
            ->nullable()	Allow NULL values to be inserted into the column
            ->storedAs($expression)	Create a stored generated column (MySQL Only)
            ->unsigned()	Set integer columns to UNSIGNED
            ->virtualAs($expression)	Create a virtual generated column (MySQL Only)

        Modifying Columns

        Prerequisites
      
      មុនពេល modify column ចូរប្រាកដវា អ្នកបានបន្ថែម doctrine/dbal dependency ទៅកាន់ composer.json file ។
Doctrine DBAL library ត្រូវបានប្រើដើម្បីកំណត់ current state នៃ column និង បង្កើត SQL queies ដែលចាំបាច់ ដើម្បី កែប្រែ column ជាក់លាក់ណាមួយ

          composer require doctrine/dbal
        
        Updating Column Attributes
        
        change method អនុញ្ញាត ឱ្យអ្នក modify column types ដែលមានស្រាប់ ទៅជាប្រភេទថ្មី ឫក៏ modify column's attributes។
អ្នកអាចបង្កើន size នៃ string colum។ ដើម្បី មើល change method action សូមមើល increase size នៃ column name ពី ២៥ ទៅ ៥០ ដូចខាងក្រោម


          Schema::table('users', function (Blueprint $table) {
              $table->string('name', 50)->change();
          });
                        
                        We could also modify a column to be nullable:

          Schema::table('users', function (Blueprint $table) {
              $table->string('name', 50)->nullable()->change();
          });
          
          column types ខាងក្រោម មិនអាច changed char double enum mediumInteger timestamp tinyInteger ipAddress json jsonb
 macAddress mediumIncrements morphs nullableMorphs nullableTimestamps softDeletes timeTz timestampTz timestamps timestampsTz
 unsigneMediumInteger unsignedTinyInteger uuid ។


                                            Renaming Columns
                                            
          ដើម្បី rename column អ្នកអាចប្រើ renameColumn method នៅលើ Schema builder ។ មុនពេល rename column 
 សូមប្រាកដថា អ្នកបាន add doctrine/dbal dependency ទៅកាន់ coposer.json file:


              Schema::table('users', function (Blueprint $table) {
                  $table->renameColumn('from', 'to');
              });
Renaming any column in a table that also has a column of type enum is not currently supported.

Dropping Columns

To drop a column, use the dropColumn method on the Schema builder. Before dropping columns from a SQLite database, you will need to add the doctrine/dbal dependency to your composer.json file and run the composer update command in your terminal to install the library:

Schema::table('users', function (Blueprint $table) {
    $table->dropColumn('votes');
});
You may drop multiple columns from a table by passing an array of column names to the  dropColumn method:

Schema::table('users', function (Blueprint $table) {
    $table->dropColumn(['votes', 'avatar', 'location']);
});
Dropping or modifying multiple columns within a single migration while using a SQLite database is not supported.

Indexes


Creating Indexes

The schema builder supports several types of indexes. First, let's look at an example that specifies a column's values should be unique. To create the index, we can simply chain the unique method onto the column definition:

$table->string('email')->unique();
Alternatively, you may create the index after defining the column. For example:

$table->unique('email');
You may even pass an array of columns to an index method to create a compound index:

$table->index(['account_id', 'created_at']);
Laravel will automatically generate a reasonable index name, but you may pass a second argument to the method to specify the name yourself:

$table->index('email', 'my_index_name');
Available Index Types

Command	Description
$table->primary('id');	Add a primary key.
$table->primary(['first', 'last']);	Add composite keys.
$table->unique('email');	Add a unique index.
$table->unique('state', 'my_index_name');	Add a custom index name.
$table->unique(['first', 'last']);	Add a composite unique index.
$table->index('state');	Add a basic index.
Index Lengths & MySQL / MariaDB

Laravel uses the utf8mb4 character set by default, which includes support for storing "emojis" in the database. If you are running a version of MySQL older than the 5.7.7 release or MariaDB older than the 10.2.2 release, you may need to manually configure the default string length generated by migrations in order for MySQL to create indexes for them. You may configure this by calling the  Schema::defaultStringLength method within your AppServiceProvider:

use Illuminate\Support\Facades\Schema;

/**
 * Bootstrap any application services.
 *
 * @return void
 */
public function boot()
{
    Schema::defaultStringLength(191);
}
Alternatively, you may enable the innodb_large_prefix option for your database. Refer to your database's documentation for instructions on how to properly enable this option.


Dropping Indexes

To drop an index, you must specify the index's name. By default, Laravel automatically assigns a reasonable name to the indexes. Simply concatenate the table name, the name of the indexed column, and the index type. Here are some examples:

Command	Description
$table->dropPrimary('users_id_primary');	Drop a primary key from the "users" table.
$table->dropUnique('users_email_unique');	Drop a unique index from the "users" table.
$table->dropIndex('geo_state_index');	Drop a basic index from the "geo" table.
If you pass an array of columns into a method that drops indexes, the conventional index name will be generated based on the table name, columns and key type:

Schema::table('geo', function (Blueprint $table) {
    $table->dropIndex(['state']); // Drops index 'geo_state_index'
});

Foreign Key Constraints

Laravel also provides support for creating foreign key constraints, which are used to force referential integrity at the database level. For example, let's define a user_id column on the posts table that references the id column on a users table:

Schema::table('posts', function (Blueprint $table) {
    $table->integer('user_id')->unsigned();

    $table->foreign('user_id')->references('id')->on('users');
});
You may also specify the desired action for the "on delete" and "on update" properties of the constraint:

$table->foreign('user_id')
      ->references('id')->on('users')
      ->onDelete('cascade');
To drop a foreign key, you may use the dropForeign method. Foreign key constraints use the same naming convention as indexes. So, we will concatenate the table name and the columns in the constraint then suffix the name with "_foreign":

$table->dropForeign('posts_user_id_foreign');
Or, you may pass an array value which will automatically use the conventional constraint name when dropping:

$table->dropForeign(['user_id']);
You may enable or disable foreign key constraints within your migrations by using the following methods:

Schema::enableForeignKeyConstraints();

Schema::disableForeignKeyConstraints();
