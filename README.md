Yii2 - jQuery DataTables 
========================
[![Latest Stable Version](https://poser.pugx.org/snickom/yii2-datatables-widget/v/stable.png)](https://packagist.org/packages/snickom/yii2-datatables-widget) [![Total Downloads](https://poser.pugx.org/snickom/yii2-datatables-widget/downloads.png)](https://packagist.org/packages/snickom/yii2-datatables-widget) [![Latest Unstable Version](https://poser.pugx.org/snickom/yii2-datatables-widget/v/unstable.png)](https://packagist.org/packages/snickom/yii2-datatables-widget) [![License](https://poser.pugx.org/snickom/yii2-datatables-widget/license.png)](https://packagist.org/packages/snickom/yii2-datatables-widget) 

Yii2 extension implements jQuery DataTables

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require --prefer-dist snickom/yii2-datatables-widget "*"
```

or add

```
"snickom/yii2-datatables-widget": "*"
```

to the require section of your `composer.json` file.


Usage
-----

Once the extension is installed, simply use it in your code by  :

```php
<?= \snickom\datatables\DynamicTable::widget([
          'id' => 'datatable-grid',
          'title' =>Yii::t('app','Table'),
          'dt'=>[
            'sDom' => '<\'datatable-header\'fl><\'datatable-scroll\'rt><\'datatable-footer\'ip>',
            'length' => 25,
            'pipelinePages' => 5,
            'scrollX' => true,
            'info' => true,
            'paging' => true,
            'searching' => true,
            'ordering' => true,
            'order' => [[0, 'asc']],
            'length' =>25,
            'lengthMenu' => [[10, 25, 50, 100], [10, 25, 50, 100]],
            'decimal' => ',',
            'thousands' => ' ',
            'stateSave' => [
              'save' =>"
                $.ajax( {
                  'url': '/state_save',
                  'data': data,
                  'dataType': 'json',
                  'method': 'POST',
                  'success': function () {}
                } );
              ",
              'load' => "
                var o;

                $.ajax( {
                  'url': '/state_load',
                  'async': false,
                  'dataType': 'json',
                  'success': function (json) {
                    o = json;
                  }
                } );

                return o;
              ",
            ],
          ],
          'db'=>[
            'table'=>'table', # !!! THIS IS REQUIRED !!! - Primary db table
            'primaryKey' =>'t.id',
            'condition' =>'',
            'searchOr' =>'',
            'searchAnd' =>'',
            'columns' =>[ # !!! THIS IS REQUIRED if your table doesn't have columns id & name !!! - Db columns to show
              [
                'db' => 't.id',
                'dt' => 't__id', 
                'type' => 'numeric',
                'title' => Yii::t('app','ID'),
              ],
              [
                'db' => 't.name',
                'dt' => 't__name',
                'title' => Yii::t('app','Name'), 
              ],
              [
                'db' => 't.id',
                'dt' => 'options',
                'title' => Yii::t('app','Options'), 
                'opt' => ['view','update','delete'], 
              ]
            ],
          ],
          'selector' => true,
          'detail' => [
          	'ajax' => '/detail/' # or /detail?id=
          ],
]); ?>
```

you can register pure assets for static tables: 


```php

\snickom\datatables\DatatableAsset::register($this);

```