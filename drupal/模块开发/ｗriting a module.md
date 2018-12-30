[TOC]

## 创建.info文件

.info文件的命名是和module的文件名字是一样的.

类似以下

```
name = Annotate
description = "Allows users to annotate nodes."
package = Pro Drupal Development
core = 7.x
files[] = annotate.module
files[] = annotate.install
files[] = annotate.admin.inc
configure=admin/config/content/annotate/settings
dependencies[] = forum
dependencies[] = taxonomy
php = 5.2
```

> 1. name是名字
> 2. description是描述
> 3. package是所在的模块的分类
> 4. core 是所属的drupal的版本
> 5. files[]一下三个表示了,这个模块有三个文件,而这个表示方法都是按照php的数组的写法来表示的.
> 6. configure是表示设置的网页连接地址
> 7. php 表示php版本
> 8. dependencies表示模块的基本的依赖

## 实现钩子函数(hook_function)

### hook_menu()函数

现在我们只需要知道这个函数是用来注册连接地址就可以的了,例子:

```php
function annotate_menu(){
    // we can access this link
    $items['admin/config/annotate'] = array(
        'title' => 'Node annotation',
        'description' => 'Adjust node annotation options.',
        'position' => 'right',
        'weight' => -5,
        'page callback' => 'system_admin_menu_block_page',
        'access arguments' =>array('administer site configuration'),
        'file' => 'system.admin.inc',
        'file path' => drupal_get_path('module','system'),
    );
    return $items;
}
```

> 记住这个函数一定要返回后面的$items这个变量,下面我们来讨论几个简单的点,并且值得注意的点
>
> 1. 注册的地址是以$items['url']的形式来进行
> 2. position:这个选项只能是'left'或者是'right',决定他系统选项页面左边或者右边
> 3. weight:决定选项的排序
> 4. page callback: 页面的回调函数名字
> 5. access arguments:接受的传入参数
> 6. file: 指定回调函数是在哪个文件当中
> 7. file path:这个文件的具体的路径,如果没有的话,默认file指定的文件路径就是当前文件目录路径

## 添加具体settings表格

```php
function annotate_admin_settings() {
// Get an array of node types with internal names as keys and
// "friendly names" as values. E.g.,
// array('page' => ’Basic Page, 'article' => 'Articles')
    // node_type_get_types() what is that about?
    $types = node_type_get_types(); // 获取所有的content type
    foreach($types as $node_type) {
        $options[$node_type->type] = $node_type->name; // 新建一个option变量来存储标量名
    }
    $form['annotate_node_types'] = array( // 创建form.具体见下面的FORM api
        '#type' => 'checkboxes', // 指定类型
        '#title' => t('Users may annotate these content types'), //标题
        '#options' => $options, // 复选框的选项值
        '#default_value' => variable_get('annotate_node_types', array('page')), //默认值
        '#description' => t('A text field will be available on these content types to
make user-specific notes.'), // 描述
    );
     $form['#submit'][] = 'annotate_admin_settings_submit'; //添加一个submit的按钮,及其调用函数
    return system_settings_form($form); //最后要用这个函数来创建这么一个表格
}
```

> 这个代码很好理解,见上面的注释
>
> [FORM API地址](https://api.drupal.org/api/drupal/developer%21topics%21forms_api_reference.html/7.x)

submit的回调函数

```php
/**
 * Process annotation settings submission.
 */
// 这个函数自动传进的有$form和$form_state两个变量
function annotate_admin_settings_submit($form, $form_state) {
    dpm(array($form,$form_state));
// Loop through each of the content type checkboxes shown on the form.
    foreach ($form_state['values']['annotate_node_types'] as $key => $value) {
// If the check box for a content type is unchecked, look to see whether
// this content type has the annotation field attached to it using the
// field_info_instance function. If it does then we need to remove the
// annotation field as the administrator has unchecked the box.
        /*
        如果没有选中,就删除掉所有的field_info_instance的实例,其中我要好好理解field	api
        */
        if (!$value) {
            $instance = field_info_instance('node', 'annotation', $key);
            if (!empty($instance)) {
                field_delete_instance($instance);
                watchdog("Annotation", 'Deleted annotation field from content type:
%key', array('%key' => $key));
            }
        } else {
// If the check box for a content type is checked, look to see whether
// the field is associated with that content type. If not then add the
// annotation field to the content type.
            /*
            如果选中了,检查对应类型的实例是否存在,理解field instance是啥
            */
            $instance = field_info_instance('node', 'annotation', $key);
            if (empty($instance)) {
                $instance = array(
                    'field_name' => 'annotation',
                    'entity_type' => 'node',
                    'bundle' => $key,
                    'label' => t('Annotation'),
                    'widget_type' => 'text_textarea_with_summary',
                    'settings' => array('display_summary' => TRUE),
                    'display' => array(
                        'default' => array(
                            'type' => 'text_default',
                        ),
                        'teaser' => array(
                            'type' => 'text_summary_or_trimmed',
                        ),
                    ),
                );
                $instance = field_create_instance($instance);
                watchdog('Annotation', 'Added annotation field to content type: %key',
                    array('%key' => $key));
            }
        }
    } // End foreach loop.
}
```

> 在这一块的话,如果要理解field的话就要看官网的field api
>
> 我觉得field的话就是我们一个content里面的一个字段.
>
> ![例如](https://ws1.sinaimg.cn/large/891f7782ly1fyp5i1mk1vj21070ly40c.jpg)

#### hook_node_load()

```php
function annotate_node_load($nodes, $types) {
    global $user;
// Check to see if the person viewing the node is the author. If not then
// hide the annotation.
    foreach ($nodes as $node) {
        if ($user->uid != $node->uid) {
            unset($node->annotation);
        }
    }
}
```

> 这个钩子函数当node被加载的时候就会运行.

