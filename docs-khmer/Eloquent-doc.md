                                                  ការណែនាំឱ្យស្គាល់ Eloquent
      Eloquent ORM គឺ featureមួយរបស់ Laravel ដែលប្រើសំរាប់ធ្វើការជាមួយ databse ៕
រាល់ table ទាំងអស់នៅក្នុង database តែងតែមាន Model មួយដែលប្រើសំរាប់ធ្វើអន្តរ ជាមួយ table ក្នុង databse។
Model អនុញ្ញាតិឱ្យយើង query  data ពី និង insert data ចូលទៅក្នុង table។
      មុនពេលអ្នកចាប់ផ្តើម ចូរប្រាកដថា អ្នកបាន  configure database connection នៅក្នុង config/databse.php។
      
                                                    ការ define Models
      បន្ទាប់ពីបានណែនាំឱ្យស្គាល់ យើងចាប់ផ្តើម បង្កើត Eloquent  Model វិញម្តង។ Models ជាទូទៅស្ថិតក្នុង directory ឈ្មោះ app ។ 
ប៉ន្តែយើងអាច ដាក់វានៅកន្លែងណាក៏បាន ដែលអាច auto-load ដោយយើងតាម composer.json file ។ គ្រប់ Eloquent Model ទាំងអស់ 
បាន extends ពី Illuminate\DatabaseEloquent\Model class ឬយើងអាចនិយាយបានម៉្យាងទៀតថា គ្រប់ Eloquent Model ទាំងអស់ derive 
ពី Illuminate\DatabseEloquent\Model class៕ 
          
        នៅក្នុង laravel យើងអាចបង្កើត model instance នីមួយៗដោយប្រើ Artisan command ដូចខាងក្រោម
            php artisan make:model <model-name> 
        
       ម្យ៉ាងវិញទៀតប្រសិនបើយើងចង់ បង្កើត migration ដំណាលគ្នាជាមួយ Model ដូចខាងក្រោម 
            php artisan make:model <model-name> -m 
      Artisan command ខាងលើ នឹង បង្កើត file 2 គឺ 
         file migration និង file model 
                    
                                                      ការបង្កើត Eloquent Model
 
      ដើម្បីងាយស្រួលយល់អំពី Model សូមមើល Model Flight ខាងក្រោមដែលយើងប្រើវាដើម្បីទាញយក information  ពី flights table នៅក្នុង database
និង  រក្សាទុក information នៅក្នុង flights table នៅក្នុង database។


          <?php
          namespace App;

          use Illuminate\Database\Eloquent\Model;

          class Flight extends Model
          {
              //
          }
          
                                              Table Names
          
      ចំណាំក្នុងករណីដែលអ្នក មិនបញ្ជាក់ឈ្មោះ table នៅក្នុង Model ។ Eloquent  នឺងប្រើ ឈ្មោះ Plural name នៃ class ក្នុង ទំរង់ "snake case" 
ជាឈ្មោះរបស់ table ។ ផ្ទុយមកវិញ Eloquent  នឹងប្រើឈ្មោះដែលដាក់ឱ្យ table ដែលគេហៅថា ដាក់ឈ្មោះ table ដោយ explicit ។
      នៅក្នុង ករណីដូច ឧទាហណ៍រខាងលើ  Eloquentនឹង សន្មតថា Flight model រក្សាទុក records នៅក្នុង flights table ៕ 
      រីឯក្នុងករណីខាងក្រោម Eloquent នឺងប្រើ ឈ្មោះ table ដែលជា field $table របស់ Flight Model ៕

            <?php

            namespace App;

            use Illuminate\Database\Eloquent\Model;

            class Flight extends Model
            {
                /**
                 * The table associated with the model.
                 *
                 * @var string
                 */
                protected $table = 'my_flights';
            }
      
                                                      Primary Keys
          Eloquent នឺង សន្មតថា table នីមួយៗ មាន primary key column ឈ្មោះ id ។ យើងអាច កំណត់ $primaryKey property ដើម្បី override 
 defualt primary key របស់ laravel។
 
          បន្ថែមលើពីនេះទៀត primary key គឺជា incrementing integer value ដែល មានន័យថា primary key នឺង cast ទៅជា int ដោយស្វ័យប្រវត្ត ៕
ប្រសិនបើយើងចង់ប្រើ non-incrementing ឬ non-numeric primary Key យើងត្រូវ set public $incrementing property នៅលើ Model ស្មើ false ៕



                                                        Timestamps
          ដោយ default Eloquent ដឹងថា create_at និង update_at column មាននៅ ក្នុង table ទាំងអស់់ក្នុង databse។
 ប្រសិនបើអ្នកមិនចង់ ឱ្យ ទាំងពីខាងលើ ត្រូវបានគ្រប់គ្រងដោយ Eloquent អ្នក ត្រូវ set $timestamps property នៅលើ model to false ។


              <?php

              namespace App;

              use Illuminate\Database\Eloquent\Model;

              class Flight extends Model
              {
                  /**
                   * Indicates if the model should be timestamped.
                   *
                   * @var bool
                   */
                  public $timestamps = false;
              }
              
          ប្រើសិនបើអ្នកត្រូវការ កែតម្រូវ format នៃ timestamps របស់អ្នក អ្នកត្រូវ set $dateFormat property នៅលើ Model របស់អ្នក ។
property នេះកំណត់ របៀបដែល date attribute ដែលត្រូវរក្សាទុកក្នុង database  ដូចនឺង format នៅពេល Model is serialized to an array or JSON


        <?php

        namespace App;

        use Illuminate\Database\Eloquent\Model;

        class Flight extends Model
        {
            /**
             * The storage format of the model's date columns.
             *
             * @var string
             */
            protected $dateFormat = 'U';
        }
        
        
        ប្រសិនបើអ្នកត្រូវការ កែតម្រូវ ឈ្មោះរបស់ colums ដែលផ្ទុក timestamps អ្នកត្រូវ set CREATE_AT និង UPDATE_AT constants នៅក្នុង Model :       


              <?php

              class Flight extends Model
              {
                  const CREATED_AT = 'creation_date';
                  const UPDATED_AT = 'last_update';
              }
                                                   
                                                   
                                                   Database Connection
        ដោយ default Eloquent model ទាំងអស់ប្រើ configure default connection  សំរាប់ application របស់អ្នក ៕ 
ប្រសិនបើអ្នកចង់ connection ជាក់លាក់ផ្សេងមួយទៀត សំរាប់ Model អ្នកត្រូវប្រើ connection property ដូចខាងក្រោម :


            <?php

            namespace App;

            use Illuminate\Database\Eloquent\Model;

            class Flight extends Model
            {
                /**
                 * The connection name for the model.
                 *
                 * @var string
                 */
                protected $connection = 'connection-name';
            }

                                                      Retrieving Models
          ដំបូងអ្នកត្រូវបង្កើត Model មួយ និង associated database table របស់វា ដើម្បីឱ្យអ្នកអាច ចាបផ្តើមទាញយក data ពី database របស់អ្នកបាន។
 បើយើងគិតទៅ Eloquent model នីមួយៗ ជា powerful query builder ដែលអនុញ្ញាតិឱ្យអ្នក ឆាប់ស្ទាត់ នឹង query database table ដែល associate ជាមួយ
 model ។ សូមមើល ឩទាហរណ៍ ដូចខាងក្រោម៖



          <?php

          use App\Flight;

          $flights = App\Flight::all();

          foreach ($flights as $flight) {
              echo $flight->name;
          }
          
          
                                              Adding Additional Constraints

          All() method របស់ Eloquent return លទ្ធផល ទាំងអស់នៅក្នុង table របស់ Model ។ តាំងពី Eloquent Model serves as query builder 
អ្នកអាចបន្ថែម constaints ទៅកាន់ queries ហើយបន្ទាប់មក ប្រើ method get() ដើម្បីទាញយកលទ្ធិផល។

            $flights = App\Flight::where('active', 1)
                           ->orderBy('name', 'desc')
                           ->take(10)
                           ->get();
                           
         តាំងពី Eloquent models គឺ query builders  អ្នកអាច review Method ទាំងអស់ ដែលអាចរកបាន នៅលើ query builder ។
អ្នកអាចប្រើ method ទាំងអស់នេះ នៅក្នុង Eloquent queries របស់អ្នក ។

                                                  Collections
          Eloquent method ដូចជា all() និង get() ដែលអាចទាញយក Multiple results ដែលជា instance 
មួយ របស់ Illuminate\Database\Eloquent\Collection នឹងត្រូវបាន return ។ Collection class បានផ្តល់នូវ method ដែលមានសារះប្រយោជន៍
ជាច្រើន សំរាប់ធ្វើការជាមួយ Eloquent results :

        $flights = $flights->reject(function ($flight) {
            return $flight->cancelled;
        });
        
        យើងអាចប្រើ loop ជាមួយ collection ដូច array ដូចខាងក្រោម ផងដែរ

          foreach ($flights as $flight) {
              echo $flight->name;
          }

                                                    Chunking Results
                                                    
            ប្រសិនបើអ្នកត្រូវការ Process thousands នៃ Eloquent recoreds អ្នកត្រូវប្រើ chunk command។ chunck() method នឺង ទាញ "chunk" នៃ 
Eloquent models  ដើម្បីឱ្យវាក្លាយជា given Closure សំរាប់ processing ។ ការប្រើប្រាស់ chunck() method នឹង ចំណាយ memory ច្រើន នៅពេល ធ្វើការជាមួយ
large result sets: 

        Flight::chunk(200, function ($flights) {
            foreach ($flights as $flight) {
                //
            }
        });
        
          argument ទីមួយ ដែលបាន pass ទៅកាន់ method chunk() ខាងលើ គឺជាចំនួន record ដែលអ្នកចង់ ទាញយក ក្នុងមួយ "chunk" ។
 Closure ដែល pass ទៅកាន់ second argument នឹងត្រូវបាន call សំរាប់ chunk ដែល ទាញពី database ។ database query នឹង execute 
 ដើម្បីទាញយក chunk នីមួយៗរបស់ records ដែល pass ទៅ Closure ។ 


                                                      Using Cursors
        cursor method អនុញ្ញាតិឱ្យអ្នក iterate database records ដោយប្រើ cursor មួយ ដែលនឹង execute single query ។
នៅពេល processing large amounts of data  cursor method អាចត្រូវបានប្រើ ដោយកាត់បន្ថយ ការប្រើប្រាស់ memory :

      foreach (Flight::where('foo', 'bar')->cursor() as $flight) {
          //
      }

                                      Retrieving Single Models / Aggregates
        បន្ថែមលើការ ទាញយក records ទាំងអស់ ពី table អ្នកអាចទាញយក  single records ដោយប្រើ find និង first ។ 
ជំនួសឱ្យការទាញយក collection មួយរបស់ models ដែល method return single model instance:
      
    public class Flight{
          public function get(){
                // Retrieve a model by its primary key...
                $flight = App\Flight::find(1);
  
                // Retrieve the first model matching the query constraints...
                $flight = App\Flight::where('active', 1)->first();
               }
     }
     
      
      យើងអាច call find method ជាមួយ array primary keys ដែល វានឹង return collection នៃគ្រប់ records ដែលមាន primary keys ស្មើ នឹង element របស់ array ដូចខាងក្រោម ៖ 
        
        ឩទាហរណ៍ : យើងចង់ស្វែងរក records នៅក្នុង table flights ដែលមាន primary keys ស្មើ ១ ឬ ២ ឬ ៣ អ្នកអាចសរសេរវាដូចខាងក្រោម ៖
          public class Flight{
              public function getCollectionOfRecords(){
                    $flights = App\Flight::find([1, 2, 3]);
                
                }
         }
         
                                                    Not Found Exceptions
         
        មានពេលខ្លះអ្នកចង់ throw exceptionមួយ ប្រើសិនបើ model មិនមាន ។ នេះជាវិធីសាស្រ្តងាយស្រួលមួយ ដែលអាចធ្វើទៅបានក្នុង routes ឬ controllers។
យើងប្រើ findOrFail and firstOrFail method ដើម្បីទាញយក first result នៃ query  ក្នុងករណីដែល result not found 
Illuminate\Database\Eloquent\ModelNotFoundException នឹងត្រូវ throw :
      
      
        $model = App\Flight::findOrFail(1);
        $model = App\Flight::where('legs', '>', 100)->firstOrFail();
        
        ប្រើសិនបើ exception មិនបាន catch  នោះ ៤០៤ http response នឹង send ដោយស្វ័យប្រវត្តមក user ។ វាគ្មានការចាំបាច់ទេដែលយើងចាំបាច់
 សរសេរ explicit check ដើម្បី return ៤០៤ response នៅពេលប្រើ method ទាំងនេះ។
        សូមមើល ឩទាហរណ៍ខាងក្រោម
           
          Route::get('/api/flights/{id}', function ($id) {
              return App\Flight::findOrFail($id);
          });

                                            Retrieving Aggregates
          យើងអាចប្រើ count,sum,max និង arregate methods ផ្សេងៗទៀតជាច្រើនដែលផ្តល់ដោយ query builder ។ 
Method ទាំងនេះ return scalar value ជំនួស full model instance ។ 


          $count = App\Flight::where('active', 1)->count();
          $max = App\Flight::where('active', 1)->max('price');

                                          Inserting & Updating Models


                                                Inserts
        ជាមួយ Model ការបង្កើត record ថ្មីមួយ  គឺជាការបង្កើត model instance ថ្មីមួយ ហើយ set attributes នៅលើ Model បន្ទាប់មក call 
save method:
        សូមមើល ឩទាហរណ៍ខាងក្រោម ៖
        

            <?php

            namespace App\Http\Controllers;

            use App\Flight;
            use Illuminate\Http\Request;
            use App\Http\Controllers\Controller;

            class FlightController extends Controller
            {
                /**
                 * Create a new flight instance.
                 *
                 * @param  Request  $request
                 * @return Response
                 */
                public function store(Request $request)
                {
                    // Validate the request...

                    $flight = new Flight;

                    $flight->name = $request->name;

                    $flight->save();
                }
            }
          
         នៅក្នុង ឩទាហរណ៍ ខាងលើ assign property name របស់  http request  អោយ name attribute  App\Flight model instance។
 នៅពែលយើង call save method  record មួយនឹង ត្រូវ insert ចូលទៅក្នុង database ។ ចំណែកឯ create_at និង update_at timestamps នឹង
 set ដោយស្វ័យប្រវត្ត នៅពេលដែល save method ត្រូវបាន call  ដោយយើងមិនចាំបាច់ set វាដោយ manuall ឡើយ ។


                                                            Updates

The save method may also be used to update models that already exist in the database. To update a model, you should retrieve it, set any attributes you wish to update, and then call the save method. Again, the updated_at timestamp will automatically be updated, so there is no need to manually set its value:

$flight = App\Flight::find(1);

$flight->name = 'New Flight Name';

$flight->save();
Mass Updates

Updates can also be performed against any number of models that match a given query. In this example, all flights that are active and have a destination of San Diego will be marked as delayed:

App\Flight::where('active', 1)
          ->where('destination', 'San Diego')
          ->update(['delayed' => 1]);
The update method expects an array of column and value pairs representing the columns that should be updated.

When issuing a mass update via Eloquent, the saved and updated model events will not be fired for the updated models. This is because the models are never actually retrieved when issuing a mass update.

Mass Assignment

You may also use the create method to save a new model in a single line. The inserted model instance will be returned to you from the method. However, before doing so, you will need to specify either a fillable or guarded attribute on the model, as all Eloquent models protect against mass-assignment by default.

A mass-assignment vulnerability occurs when a user passes an unexpected HTTP parameter through a request, and that parameter changes a column in your database you did not expect. For example, a malicious user might send an is_admin parameter through an HTTP request, which is then passed into your model's create method, allowing the user to escalate themselves to an administrator.

So, to get started, you should define which model attributes you want to make mass assignable. You may do this using the $fillable property on the model. For example, let's make the name attribute of our Flight model mass assignable:

<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    /**
     * The attributes that are mass assignable.
     *
     * @var array
     */
    protected $fillable = ['name'];
}
Once we have made the attributes mass assignable, we can use the create method to insert a new record in the database. The create method returns the saved model instance:

$flight = App\Flight::create(['name' => 'Flight 10']);
If you already have a model instance, you may use the fill method to populate it with an array of attributes:

$flight->fill(['name' => 'Flight 22']);
Guarding Attributes

While $fillable serves as a "white list" of attributes that should be mass assignable, you may also choose to use $guarded. The $guarded property should contain an array of attributes that you do not want to be mass assignable. All other attributes not in the array will be mass assignable. So,  $guarded functions like a "black list". Of course, you should use either $fillable or $guarded - not both. In the example below, all attributes except for price will be mass assignable:

<?php

namespace App;

use Illuminate\Database\Eloquent\Model;

class Flight extends Model
{
    /**
     * The attributes that aren't mass assignable.
     *
     * @var array
     */
    protected $guarded = ['price'];
}
If you would like to make all attributes mass assignable, you may define the $guarded property as an empty array:

/**
 * The attributes that aren't mass assignable.
 *
 * @var array
 */
protected $guarded = [];

Other Creation Methods

firstOrCreate/ firstOrNew

There are two other methods you may use to create models by mass assigning attributes:  firstOrCreate and firstOrNew. The firstOrCreate method will attempt to locate a database record using the given column / value pairs. If the model can not be found in the database, a record will be inserted with the given attributes.

The firstOrNew method, like firstOrCreate will attempt to locate a record in the database matching the given attributes. However, if a model is not found, a new model instance will be returned. Note that the model returned by firstOrNew has not yet been persisted to the database. You will need to call save manually to persist it
