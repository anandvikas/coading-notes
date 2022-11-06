# React rendering and re-rendering
rendering or re-rendering occurs in two phases render phase and commit phase. The commit phase is usually very fast, but rendering can be slow.

**Render phase**

 1. Find all the elements flagged for updates (In case of initial render the whole tree is flagged for update).
 2. For each flagged component the JSX is converted to react element an result is stored.
 3. Perform reconcilation - differciate b/w old and new tree a.k.a Virtual DOM.
 4. hand over the changes to the nxt phase.
 
**Commit phase**
 1. Changes are applied to actual DOM.

> note : using strict mode invokes functional components two times. But **only in development** rendering. It will not do so in production rendering. 

 
## rendering behaviour with useState hook.

usestate hook do two things.
 - update the state.
 - flag the component for update.

>  const [value, setValue] = usestate(initalValue)

"setValue" compares the previous value and the new value. If there is any differene in both then only the value is updated and the component is flagged for update. Once the component is flagged for update the re-rendring process starts.

> Note : If the same value is set after the re render then, then react will render the component for one more time and after that do not perform any subsequent re-render.

**example :**

    import  React, { useState } from  'react';
    const  UseState = () => {
    const [count, setCount] = useState(0);
    console.log('UseState render');
    return (
    <button
    onClick={() => {setCount(1);}}>{count}</button>
    );
    };
    export  default  UseState;
> Note : In case of non-premetive data types it does not compare the value but it compare the reference. See state immutability.


## rendering behaviour with useReducer hook
unlike useState, the useReducer flags the component for update every time the action is dispatched irrespective of any change in the value.

## State immutability.

In case of non-primitive data types (object, array) the useState hook doesn't compares the value but it compares the reference.
**example :**
no re-render occurs.

    import  React, { useState } from  'react';
    let  obj = { num:  1 };
    const  UseState = () => {
    const [count, setCount] = useState(obj);
    const  change = () => {
    obj.num = 2;
    setCount(obj);
    };
    console.log('UseState render');
    return  <button  onClick={change}>{count.num}</button>;
    };
    export  default  UseState;

re-render occurs.

    ...
    obj.num = 2;
    setCount({...obj});    
    ...
## Parent and child related rendering behaviour.

 - Default : Whenever parent component re-render all of its child component also re-render irrespective of any update in state in the child. And this chain goes upto the  very last child connected to tree.

 - When a component go through re-render but doesn't get commit then it is a **unnecessary re-render**.
 - If there is no state change in the child component but it need to re-render just because it's parent re-renders. In that case the re-rendering of child is unneccessary because it's re-rendering will not be commited in the DOM ( rejected in virtual DOM comparision). Unnecessary re-renders decreases the performance of the application.
 - "memo" can be used to prevent unnecessary re-renders.

> Note :In parent If the same value is set after the re render then, then react will render only parent component for one more time but do not render its child, and after that do not perform any subsequent re-render.

### Ways to prevent unnecessary re-renders.

 **1. recieve the child as a prop (children or any other) : since props are imutable it is obvious that react understands there is no change in it.**

> App.js

    import  React  from  'react';
    import  UseState  from  './component/UseState';
    import  Child1  from  './component/child1';
    
    export  default  function  App() {
	    return (
		    <div>
			    <UseState>
				    <Child1  />
			    </UseState>
		    </div>
	    );
    }

> UseState.js

    import  React, { useState } from  'react';  
    
    const  UseState = ({ children }) => {
	    const [count, setCount] = useState(1);
	    console.log('Parent render');
	    return (
		    <>
			    <button  onClick={() => {setCount(count + 1)}}>
				    {count}
			    </button>
			    {children}
		    </>
	    );
    };
    export  default  UseState;
Here child will not re-render if UseState re-renders. however the child will re-render in App component re-renders

 **2. use "memo"**

React.memo is a higher order component which checks (shallow comparision : values for primitive and reference for non-primitive.) for any update in props of the wrapped component . It only renders the wrapped component when any update in props is noticed. We can also pass a custom comparision function to the second argument of "memo"

    suppose a component chain : A => B => C(memo) => D => E

> Note :  If A re-renders then B will re-render but C,D,E will not re-render.

**example :**

    import  React  from  'react';
    const  Child1 = () => {
	    console.log('child render');
	    return  <h3>Child 1</h3>;
    };
    export  default  React.memo(Child1);

**why not to use "memo" with every component.**

 1. even with "memo" shallow comparision takes little amount of time.
 2. if props do not change , only shallow comparision time is counted.
 3. if props change , both shallow comparision and re-render time is counted.
for example : let's take same component in two cases one is memoised and another is not memoised. re-render takes 10ms , shallow comparision takes 2ms

|senario|memoised component|  non-memoised component|
|--|--|--|
| initial render | 10 |10|
| props do not change | 2 |10|
| props change | 10+2=12 |10|
| props change | 12 |10|
| props change | 12 |10|
| props change | 12 |10|
| props change | 12 |10|
| total time | 72ms |70ms|
 In the above example the performance of memoised component is less then the performance of non-memoised component.
**so, "memo" is not recommended for the components which have frequent changes in props.**

> Note: In case of object (object, array, html element, functions ) being passed as a prop a new reference is generated every time. So "memo" doing shallow comparision recognises it as a changed prop. Hence,  re-renders the component.
> To prevent this we can use "useCallback" and "useMemo" hook.
> 
**so, "memo" is not recommended for the components which is recieving object, array, html element or a function as props.**

## useCallback hook
useCallback return a memoized version of callback **function** that only changes when one of the dependencies has changed.

It is useful when passing callback function to optimised child component as a prop , that rely on reference equality to prevent unnecessary re-renders. (like "memo").

**example**

    const  increaseAge = useCallback(() => {
	    setAgeCount(ageCount + 1);
    },[ageCount]);

we can pass this memoised function ("increaseAge") as a prop and it will not undergo any reference change issues until "ageCount" is changed. Hence "memo" will not recognise it as a changed prop.

## useMemeo hook.
useMemeo return a memoized version of a value that only changes when one of the dependencies has changed.

It is useful when passing objects, arrays or html elements to optimised child component as a prop , that rely on reference equality to prevent unnecessary re-renders. (like "memo").

**example**

    
    const [name, setName] = useState("vikas")
    let  dodo = useMemo(()=>{return{ name:  name }},[name]);

we can pass this memoised object ("dodo") as a prop and it will not undergo any reference change issues until "name" is changed. Hence "memo" will not recognise it as a changed prop.

**another usecase**
since a function wrapped inside useMemo hook only runs when any update happens in the parameters provided in the dependency array. So it can also be used to prevent unnecessary invoking of an expensive function.
**example :**

    import  React, { useState, useMemo } from  'react';
    
    const  App = () => {
	    const [counter1, setCounter1] = useState(0);
	    const [number, setNumber] = useState(0);
	    let  square = useMemo(() => {
	    console.log('calculating square');
		    return  number ** 2;
	    }, [number]);
	    const  counterHander1 = () => {
		    setCounter1(counter1 + 1);
	    };
	    return (
		    <div  className="App">
		    <button  onClick={counterHander1}>Counter 1 ++</button>
		    <div>Counter 1 : {counter1}</div>
		    <input
			    type="number"
			    value={number}
			    onChange={(e) => {
			    setNumber(e.target.value);
			    }}
		    />
		    <div>square of number is : {square}</div>
		    </div>
	    );
    };
    export  default  App;
if the function to create square is not wrapped in useMemo. then "calculating square" will log every time the "counter1" or "number" changes.
**example**

    const  calcSquare = () => {
	    console.log('calculating square');
	    return  number ** 2;
    }
    let  square = calcSquare()

> If you want to cash a function : useCallback
> If you want to cash the result of a function : useMemo

## rendering behaviour related to context API.
whenever the context value is changed then the component using that context value re-renders weather it is memoised or not.
**example**

> App.js

    export  const  Context = createContext();
    export  default  function  App() {
    const [count, setCount] = useState(0);
    console.log('parent render');
    return (
    <Context.Provider  value={count}>
    <div>
    <button  onClick={() => { setCount(count + 1); }}  >{count}</button>
    <ChildA  />
    </div>
    </Context.Provider>
    );
    }

> child.js

    import React, { useContext } from 'react';
    import { Context } from './App';
    
    const ChildA = React.memo(() => {
      console.log('child A render');
      return (
        <>
          <h4>Child A</h4>
          <ChildB />
        </>
      );
    });
    
    const ChildB = () => {
      console.log('child B render');
      return (
        <>
          <h4>Child B</h4>
          <ChildC />
        </>
      );
    };
    
    const ChildC = () => {
      let value = useContext(Context);
      console.log('child C render');
      return (
        <>
          <h4>Child C : the num is {value}</h4>
        </>
      );
    };
    
    export default ChildA;

In the above example only parent and childC re-renders whenever the context changes.

Instead of wraping ChildA in React.memo we can also use "recieve the child as prop" method , that will also work fine.