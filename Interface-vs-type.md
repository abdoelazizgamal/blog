### إحنا في TypeScript عندنا طريقتين لتحديد الshape of data ممكن نستخدم types أو interfaces
### وسؤال دايمًا بيتسأل في TypeScript هو "نستخدم  (types) ولا (interfaces)؟" وإيه الفرق بينهم ؟
الإجابة على السؤال ده زي أي سؤال برمجي تاني: "على حسب". في حالات بيكون فيها واحدة أوضح وأفضل من التانية، لكن في حالات كتير تقدر تستخدم أي واحدة منهم. في المقال ده، هنتكلم عن الفروقات والتشابهات الأساسية بين الـtypes والـinterfaces، وهنشوف إمتى يكون مناسب نستخدم كل واحدة فيهم
#### نبدأ بالأساسيات
> Types and type aliases
type هو  keyword   في TypeScript نقدر نستخدمها عشان نحدد شكل البيانات. فيه basic types في TypeScript زي
*  String
* Boolean
* Number
* Array
* Tuple
* Enum
* وفيه (Advanced types) بتديك مرونة أكبر في التعامل مع البيانات
> الـType Aliases في TypeScript معناها "اسم بديل لأي نوع". يعني نقدر نسمي نوع معين باسم تاني، بس من غير ما نخلق نوع جديد. الـType Aliases مجرد اسم بديل لنوع موجود بالفعل.
ممكن نعمل Type Aliases باستخدام type keyword  وبكده نقدر نعمل اسم جديد لأي نوع في TypeScript، حتى الprimitive types 
```
type MyNumber = number;
type User = {
  id: number;
  name: string;
  email: string;
}

```

في المثال اللي فوق، عملنا Type Aliases للنوع MyNumber والنوع User 
نقدر نستخدم MyNumber كأنه نوع رقم (number)، ونستخدم User كأنه نوع بيمثل البيانات بتاعة المستخدم.
في TypeScript، الـinterface بتحدد عقد (contract) لازم الobject يلتزم بيه ، مثال
```
interface Client { 
    name: string; 
    address: string;
}
```
ممكن نعمل نفس تعريف الـClient ده باستخدام  (type)
```
type Client = {
    name: string;
    address: string;
};
```
> Differences between types and interfaces

ال (Primitive Types)
 الأنواع اللي متعارف عليها جوه TypeScript زي
* null
*  number 
*  string
*  boolean
*  undefined
ممكن نعمل Type Alias ل Primitive Type بالشكل ده
```
type Address = string;
```
لكن، مش هنقدر نستخدم الـinterface عشان نعمل Alias لPrimitive Type 
 الـinterface ينفع بس للobject   
 علشان كده لما نحتاج نعمل Alias لPrimitive Type  بنستخدم type
> ال Union types

الـUnion Types بتسمح لنا نوصف القيم اللي ممكن تكون من أكتر من type  واحد ، ممكن نعمل Union Types باستخدام ال(type) بس، مفيش حاجة زيها في الـinterface
```
type Transport = 'Bus' | 'Car' | 'Bike' | 'Walk';
```
لكن ممكن ندمج نوعين مع بعض باستخدام interfaces  زي كده باستخدام ال(type) 
```
interface CarBattery {
  power: number;
}
interface Engine {
  type: string;
}
type HybridCar = Engine | CarBattery;

```
> الFunction types
في TypeScript، الfunction type بيمثل function’s type signature ، نقدر نحدد function type باستخدام  (type) بالشكل ده
```
type AddFn =  (num1: number, num2:number) => number;

```
وممكن كمان نستخدم الـinterface عشان نمثل function type
```
interface IAdd {
   (num1: number, num2:number): number;
}
```
النوعين بيمثلوا ال function type بشكل مشابه، بس الفرق في **الsyntax** 
 ال function type بيكون أسهل في القراءة لو استخدمنا ال (type)  وكمان بيدينا إمكانيات أكتر زي Conditional Types والـMapped Types، اللي مش هنقدر نعملها بالـinterface
الـConditional Types في TypeScript بتسمح لنا بتحديد نوع معين بناءًا على شرط معين  ، يعني ممكن نحدد نوع حسب قيمة معينة أو نوع معين
مثلاً، لو عايز تعرف إذا كان النوع هو string أو number، ممكن تستخدم Conditional Types بالشكل ده
```
type IsString<T> = T extends string ? "Yes" : "No";

type Test1 = IsString<string>;  // "Yes"
type Test2 = IsString<number>;  // "No"

```

في المثال ده، IsString هو نوع يتأكد إذا كان النوع المدخل هو string ، لو كان string  النتيجة بتكون "Yes"، غير كده بتكون "No"
الـMapped Types بتسمح لنا بتعديل نوع معين بطريقة مبرمجة ، يعني نقدر نأخذ نوع معين ونطبّق عليه تعديلات مختلفة
مثلاً، لو عندك نوع يمثل مستخدم
```
type User = {
  id: number;
  name: string;
  email: string;
};
```
ممكن نستخدم Mapped Types عشان نغير كل الخصائص في النوع ده إلى optional (اختياري)
```
type PartialUser = {
  [K in keyof User]?: User[K];
};
```
في المثال ده، PartialUser هو نوع جديد يعتمد على User، بس كل خصائصه بقت optional


مثال  يجمع Conditional Types و Mapped Types

```
type TransformStringToNumber<T> = {
  [K in keyof T]: T[K] extends string ? number : T[K];
};

type Original = {
  id: number;
  name: string;
  email: string;
};

type Transformed = TransformStringToNumber<Original>;

```

 المثال ده، TransformStringToNumber هو نوع يغير كل الخصائص من string إلى number في النوع Original

 يعني النوع Transformed هيكون فيه name و email هما number بدلاً من string 

 ال Conditional Types بتسمح لنا بعمل تغييرات على النوع بناءً على شرط معين

ال Mapped Types  بتسمح لنا بتعديل كل الخصائص في نوع معين بطريقة مبرمجة

الـ type في TypeScript هو الأداة الوحيدة اللي تقدر تستخدمها لتحقيق الحاجات دي، بينما الـ interface مش بتدعم المزايا دي

>  الDeclaration Merging هي ميزة خاصة بال (interfaces) بس. نقدر نعرف interface  أكتر من مرة، وTypeScript هيجمع كل التعريفات دي في interface واحدة
```
interface Client { 
    name: string; 
}
interface Client {
    age: number;
}
const harry: Client = {
    name: 'Harry',
    age: 41
}
```

لكن الـType Aliases مش  هتقدر تعمل نفس الموضوع ده ،  لو حاولت تعرف نفس النوع (type) أكتر من مرة، هيطلعلك خطأ.


هشرح لك الفرق بين **extends** و **intersection** في TypeScript، وإزاي بتختلف طرق التعامل مع الـ Interfaces والـ Types

> **Extends** vs. **Intersection**

في TypeScript،  بتقدر تستخدم extends عشان ترث ال properties and methods من interface تانية ، يعني تقدر تضيف خصائص جديدة وتورث الخصائص الموجودة 
مثلاً، لو عندنا interface  Client وعايزين نعمل  interface  VIPClient تمدّها  ( extending ) وتضيف خصائص جديدة، نقدر نعمل كده 

```
interface Client {
    name: string;
    email: string;
}
interface VIPClient extends Client {
    benefits: string[];
}

```
هنا VIPClient هتكون عندها الخصائص بتاعة Client وكمان خاصية جديدة اسمها benefits


> الـ Intersection Operator
 لو حابب تحقق نفس النتيجة للـ type، تقدر تستخدم الـ Intersection Operator **&** يعني ندمج الـ type مع خصائص جديدة


```
type Client = {
    name: string;
    email: string;
}
type VIPClient = Client & { benefits: string[] };
```
الـ VIPClient هنا هتحتوي على خصائص الـ Client وكمان خاصية benefits

امتداد Type Alias من Interface
ممكن  كمان ت extend  ال interface من ال type بإستخدام statically known members

```
type Client = {
    name: string;
};
interface VIPClient extends Client {
    benefits: string[];
}
```

> exception  مع الـ Union Types
الـ Union Types مش ممكن ن extend  منها interface

```
type Jobs = 'salary worker' | 'retired';
interface MoreJobs extends Jobs {
  description: string;
}
```
هنا هتحصل على خطأ لأن الـ Union Type مش ثابت ومعروف أثناء الcompile  
not statically known



> الـ Type Aliases بتتعامل مع الintersection بشكل مختلف ، type alias can extend an interface using an intersection operator

```
interface Client {
    name: string;
}
Type VIPClient = Client & { benefits: string[]};
```


> **Handling conflicts when extending**
هقولك الفرق بين interfaces وtype aliases لما بيجوا في نفس property name.

لما نجي نعمل extend من interfaces، مش مسموح إننا نستخدم نفس ال**property key**، يعني لو حاولت تعمل كده هتظهرلك مشكلة زي المثال ده

```
interface Person {
  getPermission: () => string;
}

interface Staff extends Person {
   getPermission: () => string[];
}

``` 

في الحالة دي، TypeScript هيرمي Error لأنه اكتشف Conflict بين الgetPermission في الPerson والStaff.

لكن type aliases بتتعامل مع conflicts بشكل مختلف

لما تستخدم type alias وextend منه Type تاني بنفس الproperty key، هيتعامل معاه ويمدمج الproperties بدل ما يطلع Error

مثلاً، هنا بنستخدم intersection operator لدمج الgetPermission من الPerson والStaff، وtypeof operator لتحديد نوع الunion type عشان نحصل على القيمة بشكل آمن
```
type Person = {
  getPermission: (id: string) => string;
};

type Staff = Person & {
   getPermission: (id: string[]) => string[];
};

const AdminStaff: Staff = {
  getPermission: (id: string | string[]) =>{
    return (typeof id === 'string'?  'admin' : ['admin']) as string[] & string;
  }
}

```
اللي لازم تعرفه إن دمج الtype intersection بين خاصيتين ممكن يديك نتائج غير متوقعة مثلاً، لو حاولت تدمج خاصية اسمها name تكون string مع number في type جديد، هتواجه مشكلة زي دي:

```
type Person = {
    name: string
};

type Staff = Person & {
    name: number
};
// error: Type 'string' is not assignable to type 'never'.(2322)
const Harry: Staff = { name: 'Harry' };

```
في المثال ده
هنا، name في النوع Staff لازم يكون string و number في نفس الوقت، وده مستحيل. عشان كده، TypeScript هيستخدم النوع never ويوريك
خطأ لما تحاول تعين قيمة string لـ name. 
ببساطة، never بيستخدم للدلالة على الحالات اللي مش ممكن تتحقق، وده بيساعد TypeScript في تحديد الأخطاء ومنعها في الكود.


في النهاية، الinterfaces هتكتشف conflicts بين الproperty أو method names في وقت الcompile وتطلع Error، في حين الtype intersections هتدمج الproperties أو methods من غير ما تطلع Errors. لو كنت محتاج overload للfunctions، يبقى الأحسن تستخدم type aliases.









