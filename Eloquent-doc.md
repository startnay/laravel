                                                  ការណែនាំឱ្យស្គាល់ Eloquent
      Eloquent ORM គឺ featureមួយរបស់ Laravel ដែលប្រើសំរាប់ធ្វើការជាមួយ databse ៕
រាល់ table ទាំងអស់នៅក្នុង database តែងតែមាន Model មួយដែលប្រើសំរាប់ធ្វើអន្តរ ជាមួយ table ក្នុង databse។
Model អនុញ្ញាតិឱ្យយើង query  data ពី និង insert data ចូលទៅក្នុង table។

      មុនពេលអ្នកចាប់ផ្តើម ចូរប្រាកដថា អ្នកបាន  configure database connection នៅក្នុង config/databse.php។
      
                                                    ការបង្កើត Models
      បន្ទាប់ពីបានណែនាំឱ្យស្គាល់ យើងចាប់ផ្តើម បង្កើត Eloquent  Model វិញម្តង។ Models ជាទូទៅស្ថិតក្នុង directory ឈ្មោះ app ។ 
ប៉ន្តែយើងអាច ដាក់វានៅកន្លែងណាក៏បាន ដែលអាច auto-load ដោយយើងតាម composer.json file ។ គ្រប់ Eloquent Model ទាំងអស់ 
បាន extends ពី Illuminate\DatabaseEloquent\Model class ឬយើងអាចនិយាយបានម៉្យាងទៀតថា គ្រប់ Eloquent Model ទាំងអស់ derive 
ពី Illuminate\DatabseEloquent\Model class៕ 
      
      To get started, let's create an Eloquent model. Models typically live in the app directory,
 but you are free to place them anywhere that can be auto-loaded according to your composer.json file. 
 All Eloquent models extend Illuminate\Database\Eloquent\Model class.
      
        នៅក្នុង laravel យើងអាចបង្កើត model instance នីមួយៗដោយប្រើ Artisan command ដូចខាងក្រោម
            php artisan make:model <model-name> 
            

      
