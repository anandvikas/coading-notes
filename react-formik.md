## Formik

    npm install formik

 - **values** : object containing form values
 - **handleChange** : listen for any change in the form field
 - **handleSubmit** : handles the submit of form
 - **errors** : contains fields which have errors with their error messages.
 - **touched** : gives information of the fields which are already visited (it will help to show error messages of only those fields which has been interacted with)
 - **handleBlur** : It will check weather the field is unfocused after being focused, and if so -- it will add that field in touched object.
 - **getFieldProps**: binds fiormik related props in a single object. (can be used to make code smaller)
 - 
 

    import React from 'react';
    import { useFormik } from 'formik';
    
    export default function Form1() {
      const formik = useFormik({
        initialValues: {
          name: '',
          email: '',
          age: '',
          pass: '',
          rePass: '',
          car: 'audi',
        },
        onSubmit: (values) => {
          console.log(values);
        },
        validate: (values) => {
          let errors = {};
          // ---------------------------------------------
          if (!values.name) {
            errors.name = 'name is required';
          }
          // ---------------------------------------------
          if (!values.email) {
            errors.email = 'email is required';
          } else if (!values.email.match(/^[^\s@]+@[^\s@]+\.[^\s@]+$/)) {
            errors.email = 'Invalid Email format';
          }
          // ---------------------------------------------
          if (!values.age) {
            errors.age = 'age is required';
          }
          // ---------------------------------------------
          if (!values.pass) {
            errors.pass = 'password is required';
          }
          // ---------------------------------------------
          if (!values.rePass) {
            errors.rePass = 'repeat password is required';
          }
          // ---------------------------------------------
    
          return errors;
        },
      });
    
      let { values, handleChange, handleSubmit, errors, touched, handleBlur } = formik;
    
      return (
        <div>
          <form onSubmit={handleSubmit}>
            <input
              type="text"
              name="name"
              placeholder="Name"
              onBlur={handleBlur}
              onChange={handleChange}
              value={values.name}
            />
            <span>{touched.name && errors.name && errors.name}</span>
            <br />
            <input
              type="text"
              name="email"
              placeholder="Email"
              onBlur={handleBlur}
              onChange={handleChange}
              value={values.email}
            />
            <span>{touched.email && errors.email && errors.email}</span>
            <br />
            <input
              type="number"
              name="age"
              placeholder="Age"
              onBlur={handleBlur}
              onChange={handleChange}
              value={values.age}
            />
            <span>{touched.age && errors.age && errors.age}</span>
            <br />
            <input
              type="password"
              name="pass"
              placeholder="Password"
              onBlur={handleBlur}
              onChange={handleChange}
              value={values.pass}
            />
            <span>{touched.pass && errors.pass && errors.pass}</span>
            <br />
            <input
              type="password"
              name="rePass"
              placeholder="Repeat Password"
              onBlur={handleBlur}
              onChange={handleChange}
              value={values.rePass}
            />
            <span>{touched.rePass && errors.rePass && errors.rePass}</span>
            <br />
            <select name="car" onChange={handleChange} value={values.car}>
              <option value="bmw">BMW</option>
              <option value="audi">Audi</option>
              <option value="fiat">Fiat</option>
            </select>
            <br />
            <input type="submit" value="submit form" />
          </form>
        </div>
      );
    }

## Validation with "YUP"

    npm install yup

 - code
 - 

    ...
    import * as Yup from 'yup';
    
    export default function Form1() {
      const validationSchema = Yup.object({
        name: Yup.string().required('Name is required'),
        email: Yup.string()
          .email('Invalid email format')
          .required('Email is required'),
        age: Yup.number().required('Age is required'),
        pass: Yup.string()
	        .required('Password is required')
	        .min(8, 'Password is too short - should be 8 chars minimum.')
	        .matches(/^(?=(.*[a-z]){1,})(?=(.*[A-Z]){1,})(?=(.*[0-9]){1,})(?=(.*[!@#$%^&*()\-__+.]){1,}).{8,}$/, 'Password must contain Uppercase, lowercase, number and special charecter.')
        rePass: Yup.string().required('Repeat password is required'),
      });
      const formik = useFormik({
        initialValues: {
          name: '',
          email: '',
          age: '',
          pass: '',
          rePass: '',
          car: 'audi',
        },
        onSubmit: (values) => {
          console.log(values);
        },
        validationSchema,
      });
      ...
    
      return (
        ...
      );
    }

##  getFieldProps()

    ...
    import * as Yup from 'yup';
    
    export default function Form1() {
      ...
      const formik = useFormik({
        initialValues: {
          name: '',
          email: '',
          age: '',
          pass: '',
          rePass: '',
          car: 'audi',
        },
        onSubmit: (values) => {
          console.log(values);
        },
        validationSchema,
      });
    
      let { handleSubmit, errors, touched, getFieldProps } = formik;
    
      return (
        <div>
          <form onSubmit={handleSubmit}>
            <input type="text" placeholder="Name" {...getFieldProps('name')} />
            <span>{touched.name && errors.name && errors.name}</span>
            <br />
            <input type="text" placeholder="Email" {...getFieldProps('email')} />
            <span>{touched.email && errors.email && errors.email}</span>
            <br />
            <input type="number" placeholder="Age" {...getFieldProps('age')} />
            <span>{touched.age && errors.age && errors.age}</span>
            <br />
            <input
              type="password"
              placeholder="Password"
              {...getFieldProps('pass')}
            />
            <span>{touched.pass && errors.pass && errors.pass}</span>
            <br />
            <input
              type="password"
              placeholder="Repeat Password"
              {...getFieldProps('rePass')}
            />
            <span>{touched.rePass && errors.rePass && errors.rePass}</span>
            <br />
            <select {...getFieldProps('car')}>
              <option value="bmw">BMW</option>
              <option value="audi">Audi</option>
              <option value="fiat">Fiat</option>
            </select>
            <br />
            <input type="submit" value="submit form" />
          </form>
        </div>
      );
    }
## Formik, Form, Field component

**Formik** : it replaces the use of useFormik and takes *initialValues*, *validationSchema*, *onSubmit* as props.
**Form**: it eliminates the use of *handleSubmit*.
**Field**: it eliminates the use of *getFieldProps*. (but requires name attrebute)
**ErrorMessage**: It eleminates the use use of extra html element for displaying errors. It gives all features of *touched*, *errors* and takes a name prop whose error is to be displayed.

    import React from 'react';
    import { Formik, Form, Field, ErrorMessage } from 'formik';
    import * as Yup from 'yup';
    
    export default function Form2() {
      const initialValues = {
        name: '',
        email: '',
        age: '',
        pass: '',
        rePass: '',
        car: 'audi',
      };
    
      const onSubmit = (values) => {
        console.log(values);
      };
    
      const validationSchema = Yup.object({
        name: Yup.string().required('Name is required'),
        email: Yup.string()
          .email('Invalid email format')
          .required('Email is required'),
        age: Yup.number().required('Age is required'),
        pass: Yup.string().required('Password is required'),
        rePass: Yup.string().required('Repeat password is required'),
      });
    
      return (
        <Formik
          initialValues={initialValues}
          onSubmit={onSubmit}
          validationSchema={validationSchema}
        >
          <Form>
            <Field type="text" placeholder="Name" name="name" />
            <ErrorMessage name="name" />
            <br />
            <Field type="text" placeholder="Email" name="email" />
            <ErrorMessage name="email" />
            <br />
            <Field type="number" placeholder="Age" name="age" />
            <ErrorMessage name="age" />
            <br />
            <Field type="password" placeholder="Password" name="pass" />
            <ErrorMessage name="pass" />
            <br />
            <Field type="password" placeholder="Repeat Password" name="rePass" />
            <ErrorMessage name="rePass" />
            <br />
            <input type="submit" value="submit form" />
          </Form>
        </Formik>
      );
    }

## rendering textarea

    <Field  as="textarea"  type="text"  placeholder="Comment"  name="comment"  />    
    <ErrorMessage  name="comment"  />
## render custom input field using Field component (render-props)

    <Field name="class">
      {(props) => {
        // props contain all values and methods require to configure the custom input elment
        console.log(props);
        let { field, form, meta } = props;
        return (
          <div>
            <input placeholder="Class" {...field} />
            <span>{meta.touched && meta.error && meta.error}</span>
          </div>
        );
      }}
    </Field>
OR

    <Field name="class">
      {(props) => {
        // props contain all values and methods require to configure the custom input elment
        console.log(props);
        let { field, form, meta } = props;
        return (
          <div>
            <input placeholder="Class" {...field} />
            <ErrorMessage name="class" />
          </div>
        );
      }}
    </Field>
## ErrorMessage component prop

by default error message is rendered as a plain text without wrapper inside any html element. But we can customise it.

> custom component

    const  ErrorComp = (props) => {
	    return  <span  style={{color:"red"}}>{props.children}</span>
    }

> ErrorMessage

    <ErrorMessage  name="comment"  component={ErrorComp}/>
### custom error message using render-prop

    <ErrorMessage  name="pass">
	    {(msg) =>  <span  style={{ color:  'red' }}>{msg}</span>}
    </ErrorMessage>
## nested objects and arrays

    import React from 'react';
    import { Formik, Form, Field, ErrorMessage } from 'formik';
    import * as Yup from 'yup';
    
    const ErrorComp = (props) => {
      return <span style={{ color: 'red' }}>{props.children}</span>;
    };
    
    export default function Form2() {
      const initialValues = {
        ...
        address: {
          state: '',
          city: '',
        },
        phone: ['', ''],
      };
    
      const onSubmit = (values) => {
        console.log(values);
      };
    
      const validationSchema = Yup.object({
        ...
        address: Yup.object({
          state: Yup.string().required('State is required'),
          city: Yup.string().required('city is required'),
        }),
        // validation of array is yet to learn
      });
    
      return (
        <Formik
          initialValues={initialValues}
          onSubmit={onSubmit}
          validationSchema={validationSchema}
        >
            ...
    
            <Field type="text" placeholder="State" name="address.state" />
            <ErrorMessage name="address.state" />
            <br />
            <Field type="text" placeholder="City" name="address.city" />
            <ErrorMessage name="address.city" />
            <br />
    
            <Field type="number" placeholder="Phone 1" name="phone[0]" />
            <br />
            <Field type="number" placeholder="Phone 2" name="phone[1]" />
            <br />
    
            <input type="submit" value="submit form" />
          </Form>
        </Formik>
      );
    }
 