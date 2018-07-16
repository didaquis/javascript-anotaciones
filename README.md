# 📚 JavaScript: breves anotaciones sobre el lenguaje  

Listado personal de anotaciones, trucos, recordatorios, utilidades o ejemplos interesantes para JavaScript.  

## Tabla de Contenido
- [Declaración de variables](#declaracion-de-variables)
- [Operadores](#operadores)
- [Funciones y argumentos](#funciones-y-argumentos)
- [Tratamiento de arrays](#tratamiento-de-arrays)
- [Tratamiento de strings](#tratamiento-de-strings)
- [Objetos](#objetos)
- [Set](#set)
- [Map](#map)
- [Debugging y console](#debugging-y-console)


----------------------------------------------------------
## Declaración de variables:  

* Declarar múltiples variables de manera más compacta:
```javascript
var i, j, k;
var l = m = n = 0;
var a = 1, b = 2;
```

----------------------------------------------------------
## Operadores:  

* Operador lógico _OR_ (evalua de izquierda a derecha las expresiones y retorna la primera de ellas evaluable como "truthy". Si todas se evaluan como "falsy", retorna _false_. Todos los valores son "truthy", excepto: _false_, _0_, _""_, _null_, _undefined_, y _NaN_)
```javascript
var foo = false || ':-)'; // ':-)'


var bar = 3 || false; // 3


var foobar = null || 0 || undefined || "NyanCat"; // "NyanCat"
```

* Operador ternario: _?_:
```javascript
var now = new Date();  
var greeting = "Good" + ((now.getHours() > 17) ? " evening." : " day.");
console.log(greeting);
```

* Operador _spread_:
```javascript
let foo = ['foo 1', ' foo 2'];
let bar = ['bar 1', 'bar 2', ...foo, 'bar 3'];
console.log(bar); // ["bar 1", "bar 2", "foo 1", " foo 2", "bar 3"]
```

* Doble uso del operador _!_ (not):
```javascript
// La doble negación retorna un booleano dependiendo de la "truthiness" de la expressión. Retornará false cuando el valor sea: null, undefined, 0, etc... Si no es ninguno de estos casos, entonces devolverá true. Tiene sentido usarlo para evaluar expressiones que no son booleanas.
let foo;
if (!!foo) { // 'Result is false' Dado que el valor de foo es null.
	console.log('Result is true');
} else {
	console.log('Result is false');
}

// Los siguientes ejemplos se evaluan como ciertos:
!!0 // false
!!1 // true
!!"" // false (string vacío es falsy)
!!window.biz // false (undefined es falsy)
!!null // false (null is falsy)
!!{} // true (an empty object is truthy)
!![] // true (an empty array is truthy. PHP programmers beware!)
```

----------------------------------------------------------
## Funciones y argumentos:  

* _arguments_ (array de argumentos que se le pasan a una función):
```javascript
function codeScript(){
	for(var j = 0; j <= arguments.length -1; j++){
		console.log(arguments[j]);
	}
}
codeScript(3712,7123);
```

* Establecer un valor por defecto a los argumentos de una función (si no pasamos un parámetro, JavaScript lo interpreta como "undefined"):
```javascript
function valorPorDefectoEnUnArgumento(arg){
	// siempre que "arg" sea "undefined", "arg" será igual a "valor por defecto"
	var arg = arg || "valor por defecto";
	
	console.log(arg);
}

valorPorDefectoEnUnArgumento("foo"); // "foo"
valorPorDefectoEnUnArgumento(); // "valor por defecto"
```

* Self Invoking Functions:
```javascript
var hello;
(function (){

	hello = function(){
		return "Hi boy!";
	};

})();

console.log( hello() ); // "Hi boy!"
```

----------------------------------------------------------
## Tratamiento de arrays:

* Clonar un array
```javascript
/**
 * recursiveArrayClone() Crea un clon exacto de un array. Soporta matrices multidimensionales.
 *
 * @code
 * 	console.log( recursiveArrayClone( ['a', {}, [4, 0]] ) ); // ['a', {}, [4, 0]]
 * @endcode
 *
 * @param {array} arrayToClone. El array que se quiere clonar.
 * @return {array} clonedArray. Retorna un nuevo array.
 */
function recursiveArrayClone(arrayToClone){
	var clonedArray = [];
	for (var i = 0; i < arrayToClone.length; i++) {
		if( Array.isArray(arrayToClone[i]) ){
			clonedArray.push( recursiveArrayClone(arrayToClone[i]) );
		}else{
			clonedArray.push(arrayToClone[i]);
		}
	}
	return clonedArray;
}
```

* Vaciar un array
```javascript
var myArray = ['a', 'b', 'c'];
myArray.length = 0;
console.log(myArray); // []
```

* Uso de: _array.join_ (retorna un string)
```javascript
var a = ['Wind', 'Rain', 'Fire'];
a.join();      // 'Wind,Rain,Fire'
a.join('');    // 'WindRainFire'


var text = "Add the potato please, I always prefer more potato";
text = text.split("potato").join("cheese");
// Sustituye "potato" por "cheese" en todas las apariciones que haya en "text"
```

* Uso de: _array.concat_ (crea un nuevo array con la concatenación de 2 o más arrays)
```javascript
var arr1 = ['a', 'b', 'c'];
var arr2 = ['d', 'e', 'f'];
var arr3 = arr1.concat(arr2); // arr3 is a new array [ "a", "b", "c", "d", "e", "f" ]
```

* Uso de: _array.slice_ (crea una copia del array pero quitando, o no, algunos elementos del original)
```javascript
var fruits = ['Banana', 'Orange', 'Lemon', 'Apple', 'Mango'];
var citrus = fruits.slice(1, 3); // citrus == ['Orange','Lemon']


var reduced_fruits = fruits.slice(1); // reduced_fruits == ['Orange', 'Lemon', 'Apple', 'Mango']
```

* Uso de: _array.splice_ (permite añadir o eliminar elementos a un array. También es útil para partir un array en dos)
```javascript
var instruments = ['guitar', 'bass', 'saxophone'];
instruments.splice(2, 0, 'drums'); // instruments == ['guitar', 'bass', 'drums','saxophone']


var planets = ['Mercury','Venus','Earth','Mars','Pluto','Jupiter','Saturn','Uranus','Neptune'];
planets.splice(4,1);
// (sorry, Pluto) planets == ['Mercury','Venus','Earth','Mars','Jupiter','Saturn','Uranus','Neptune']


array_example.splice(0, 0, "foo"); // Añade "foo" a la primera posición de un array


var myFish = ['angel', 'clown', 'mandarin', 'sturgeon', 'cherubfish'];
var removedFish = myFish.splice(2);
console.log(myFish); // ["angel", "clown"]
console.log(removedFish); // ["mandarin", "sturgeon", "cherubfish"]
```

* Uso de: _array.indexOf_ (busca un determinado elemento en un array)
```javascript
var a = ["foo", "bar", "biz"]; 
a.indexOf("bar"); // 1 
a.indexOf("NyanCat"); // -1

if (a.indexOf("NyanCat") === -1) {
  // element doesn't exist in array
}
```

* Uso de: _array.filter_ (crea un nuevo array a partir de otro con aquellos elementos que pasan el filtro especificado)
```javascript
function isBigEnough(value) {
  return value >= 10;
}
var filtered = [12, 5, 8, 130, 44].filter(isBigEnough); // filtered is [12, 130, 44]


var numbers = [12, 5, 8, 130, 44];
var filteredNumbers = numbers.filter(
		function(oneNumber) {
			return oneNumber >= 10;
		}
	); // filteredNumbers is [12, 130, 44]
```

* Uso de: _array.map_ (crea un nuevo array a partir de realizar una función en cada uno de los elementos del array original)
```javascript
var numbers = [1, 4, 9];
var roots = numbers.map(Math.sqrt); // roots is now [1, 2, 3]


var nums = [1,2,3];
var doubleNums = nums.map(
	function(number){
		return number * 2;
	}
);
// doubleNums is now [2, 4, 6]
```

* Uso de: _array.forEach_ (recorre el array aplicando una función a cada elemento)
```javascript
var a = ['a', 'b', 'c'];

a.forEach(function(element) {
	console.log(element);
});


var arrayOfObjects = [ { "id": 3, "name": "Laura"}, { "id": 8, "name": "Javier"}]
var results = "";
arrayOfObjects.forEach(
	function(objectInArray){
		return results += objectInArray["name"];
	}
);
console.log(results); // "LauraJavier"
```

* Uso de: _array.reduce_ (aplica una función a un acumulador y a cada valor de un array (de izquierda a derecha) para reducirlo a un único valor.)
```javascript
cart = [ {ref: "yuh", price: 23}, {ref: "jum", price: 44} ];
var result = cart.reduce(function(accum, v){
	return accum + v.price;
},0); // en este caso, el 0 es el acumulador.
console.log(result); // 67


var arrayOfBooleans = [true, false, true, true];
var totalOfTrues = arrayOfBooleans.reduce( function(accumulator, arrayElement){
			return arrayElement ? ++accumulator : accumulator;
		},0
	);
console.log(totalOfTrues); // 3
```

----------------------------------------------------------
## Tratamiento de strings:  

* Uso de: _string.match_ (crea un array a partir de comparar un string con una regular expression)
```javascript
var str = "The rain in SPAIN stays mainly in the plain"; 
var res = str.match(/ain/gi); // ["ain","AIN","ain","ain"]


var text = "The phone number is 93 555 22 33";
var result = text.match(/\d+/); // ["93"]
var allResults = text.match(/\d+/g); // allResults is ["93", "555", "22", "33"]
var numberOfResults = text.match(/\d+/g).length; // 4


var text = "<p>Hello {{name}}, you are {{age}} years old.</p>";
var extract = text.match(/\{\{(.+?)\}\}/g);
console.log(extract); // ["{{name}}", "{{age}}"]
```

* Uso de: _string.search_ (retorna un boolean si encuentra una coincidencia en el string con el patrón indicado)
```javascript
var str = "The best things in life are free";
var pattern = new RegExp(/life/g);
var res = pattern.test(str); // true
```

* Diferencias entre _string.substr_ y _string.substring_ (ambos métodos retornan un nuevo string a partir del original, pero observa la diferencia de resultados):
```javascript
var text = "Example";
console.log(text.substr(1,5)); // "xampl" 
// (a partir del índice dado, retorna tantos caracteres como indiques en el segundo parámetro)


console.log(text.substring(1,5)); // "xamp" 
// (a partir del índice dado, retorna caracteres hasta el otro índice dado)
```

* Uso de: _string.split_ (genera un nuevo array a partir de trocear un string por el parámetro indicado)
```javascript
var array_of_words = "A new car".split(" ");
console.log(array_of_words); // ["A", "new", "car"]


console.log( "hello".split("") ); // ["h", "e", "l", "l", "o"]


var text = "Add the potato please, I always prefer more potato";
text = text.split("potato").join("cheese");
// Sustituye "potato" por "cheese" en todas las apariciones que haya en "text"
```

----------------------------------------------------------
## Objetos:

* Contexto de _this_:
```javascript
var biz = {
	nombre: "Pedro",

	// error, "this" apunta al objecto global:
	saludar: 'Hola, soy ' + this.nombre,

	// correcto, "this" apunta a "biz":
	presentarse: function(){
		return 'Mi nombre es ' + this.nombre;
	}
}

console.log(biz.saludar); // 'Hola, soy undefined'
console.log(biz.presentarse()); // 'Mi nombre es Pedro'
```

* Obtener un array a partir de las propiedades de un objeto:
```javascript
var foo = {
	nombre: 'Juan',
	apellido: 'Ruiz'
}

var arrayOfProperties = Object.keys(foo);
console.log(arrayOfProperties); // ['nombre', 'apellido']
```

* Crear un array a partir de un objeto:
```javascript
var biz = {
	length: 2,
	'0': 'biz',
	'1': 'foo'
}

var arrayFromObject = Array.prototype.slice.call(biz);
console.log(arrayFromObject); // ['biz', 'foo']
console.log(arrayFromObject.length); // 2
console.log(arrayFromObject[1]); // 'foo'
```

* Clonar un objeto:
```javascript
function deepClone(originalObject) {
	const clonedObject = {};
	for (var key in originalObject) {
		if (typeof (originalObject[key]) != 'object') {
			clonedObject[key] = originalObject[key];
		} else {
			clonedObject[key] = deepClone(originalObject[key]);
		}
	}
	return clonedObject;
}

const newObject = deepClone(originalObject);
```

----------------------------------------------------------
## Set: 

* Descomponer strings usando _set_ (recuerda que no permite valores duplicados):
```javascript
let foooooooooo = new Set('foooooooooo');
console.log(foooooooooo); // Set(2) {"f", "o"}
```

* Eliminar elementos duplicados en un array mediante _set_ y _spread_ operator:
```javascript
let nums = [1, 2, 2, 2, 2, 3, 4];
function deleteDuplicated(items) {
	return [ ... new Set(items) ];
}
console.log( deleteDuplicated(nums) ); // [1, 2, 3, 4]
```

----------------------------------------------------------
## Map: 

* Primeros pasos con _map_:
```javascript
const biz = new Map();
biz.set('name', 'John');
biz.set('age', 53);

console.log(biz.size); // 2

for (let [key, value] of biz) {
  console.log(`${key} => ${value}`);
  // name => John
  // age => 53
}

console.log(biz.get('age')); // 53

biz.delete('age'); // true
console.log(biz); // {"name" => "John"}

biz.clear(); // {}
```

* Más opciones interesantes con _map_:
```javascript
const biz = new Map( [ ['name','John'], ['Surname', 'Doe'], [undefined, null] ] );

console.log(biz.has('name')); // true
console.log(biz.has('Give me a cookie')); // false

console.log(biz); // {"name" => "John", "Surname" => "Doe", undefined => null}

const arrayOfKeys = Array.from(biz.keys()); // ["name", "Surname", undefined]

biz.forEach( (value, key, originalMap) => {
	if(value === null) {
		originalMap.delete(key);
	}
});
console.log(biz); // {"name" => "John", "Surname" => "Doe"}

biz.set('name', 'Maria'); // {"name" => "Maria", "Surname" => "Doe"}
```

----------------------------------------------------------
## Debugging y console:

* Uso del comando _debugger_ (establece un "breakpoint" en el código):
```javascript
// some code...
debugger;
// some code...
```

* La propiedad _table_  del objeto _console_ (retorna una tabla a partir de un objeto o un array)
```javascript
var person = { firstName:"John", lastName:"Doe", age:50 };
console.table(person);
```

* Destructuring en el _console.log_
```javascript
let person = { firstName:"John", lastName:"Doe", age:34 };
console.log( { person } );
```

* Uso de _console.group()_ / _console.groupEnd()_ en bucles:
```javascript
for(var i = 0; i < 2; i++){
	console.group("Primer nivel: ", i);

	for(var k = 0; k < 2; k++){
		console.group("Segundo nivel: ", k);
		
		// Your code...
		console.log("Value of i: " + i);
		console.log("Value of k: " + k);

		console.groupEnd();
	}

	console.groupEnd();
}
```

* Uso de _console.log_ con estilos CSS:
```javascript
var foo = "AWESOME";
console.log('%c' + foo, 'color: yellow; background-color: black;');
```

* Uso de _console.error_:
```javascript
console.error("Algo salió mal");
```

* Un pequeño truco para saber con que tipo de objeto trabajamos:
```javascript
function classOf(element){
	if(element === null){
		return "Null";
	}
	if(element === undefined){
		return "Undefined";
	}
	return Object.prototype.toString.call(element).slice(8,-1);
}

console.log( classOf( ["A", "B"] ) ); // "Array"
console.log( classOf( "foo" ) ); // "String"
console.log( classOf( 4 ) ); // "Number"
console.log( classOf( /[a]/g ) ); // "RegExp"
```

----------------------------------------------------------


