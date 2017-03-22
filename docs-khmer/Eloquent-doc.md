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
      
