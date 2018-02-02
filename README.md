## code
```sh
<?php
$getUserName = function($v){
    $v['userName'] = $v['author']['firstName'] .' '.$v['author']['lastName'];
    return $v;
};

$a = AH()
    ->to('posts',[  
        AH()->filter( AH()->propEq('publish',true) ),
        AH()->map([
            $getUserName,
            AH()->unpick(['pageView','author','publish'])])
            ])
    ->to('posts/#.*#/comments',[
        AH()->take(2)->map([
            $getUserName,
            AH()->pick(['userName','pageView'])
        ])
    ]);
    
$addSmile = AH()->to('posts/#.*#/comments/#.*#/userName',function($v){return $v.' !^_^';});

print_r($a->combine($addSmile)->getValue($data));
?>
```

Use the function on data:
```sh
<?php
$data = [
    'posts'=>[
        ['content'=>'p0',
         'author' => ['firstName'=>'joe','lastName'=>'lee'],
         'pageView'=>10,
         'publish'=>true,
         'comments'=>[
                ['author' => ['firstName'=>'sam','lastName'=>'wong'],
                'content'=>'com0.0',
                'pageView' => 2,
                'publish'=>true
                ],[
                    'author' => ['firstName'=>'john','lastName'=>'li'],
                    'content'=>'com0.1',
                    'pageView' => 3,
                    'publish'=>false
                ],[
                    'author' => ['firstName'=>'john','lastName'=>'li'],
                    'content'=>'com0.1',
                    'pageView' => 3,
                    'publish'=>false
                ],[
                    'author' => ['firstName'=>'john','lastName'=>'li'],
                    'content'=>'com0.1',
                    'pageView' => 3,
                    'publish'=>false
                ],[
                    'author' => ['firstName'=>'john','lastName'=>'li'],
                    'content'=>'com0.1',
                    'pageView' => 3,
                    'publish'=>false
                ]
            ]
         ],[
            'content'=>'p1',
            'author' => ['firstName'=>'zoe','lastName'=>'chan'],
            'pageView'=>20,
            'publish'=>true,
            'comments'=>[
                [
                'author' => ['firstName'=>'ken','lastName'=>'chan'],
                'content'=>'com1.0',
                'pageView' => 10,
                'publish'=>true
                ],
            ]
            ],[
            'content'=>'p1',
            'author' => ['firstName'=>'zoe','lastName'=>'chan'],
            'pageView'=>20,
            'publish'=>false,
            'comments'=>[
                [
                'author' => ['firstName'=>'ken','lastName'=>'chan'],
                'content'=>'com1.0',
                'pageView' => 10,
                'publish'=>true
                ],
            ]
        ]
    ],
];
```
## Outputs:
```sh
Array
(
    [posts] => Array
        (
            [0] => Array
                (
                    [content] => p0
                    [comments] => Array
                        (
                            [0] => Array
                                (
                                    [pageView] => 2
                                    [userName] => sam wong !^_^
                                )

                            [1] => Array
                                (
                                    [pageView] => 3
                                    [userName] => john li !^_^
                                )
                        )
                    [userName] => joe lee
                )
            [1] => Array
                (
                    [content] => p1
                    [comments] => Array
                        (
                            [0] => Array
                                (
                                    [pageView] => 10
                                    [userName] => ken chan !^_^
                                )
                        )
                    [userName] => zoe chan
                )
        )
)
```

