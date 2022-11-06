# React hook form
## Basic Form (register, handleSubmit)

    import  React  from  'react'
    import { useForm } from  "react-hook-form" 
    
    const  App = () => {
	    const { register, handleSubmit } = useForm()
	    const  submitFun = (data) => {
		    console.log(data)
	    }
	    return (
		    <div>
			    <form  onSubmit={handleSubmit(submitFun)}>
				    <div>
					    <input  type='text'  placeholder='User Name'  {...register('userName')}  />
					    <input  type='email'  placeholder='Email-ID'  {...register('email')}  />
					    <input  type='password'  placeholder='Password'  {...register('password')}  />
					    <input  type='number'  placeholder='Age'  {...register('age')}  />
				    </div>
				    <div>
					    <input  type='radio'  value='male'  {...register('gender')}  />
					    <input  type='radio'  value='female'  {...register('gender')}  />
					    <input  type='radio'  value='other'  {...register('gender')}  />
				    </div>
				    <div>
					    <input  type='checkbox'  {...register('isAdult')}  />
					    <input  type='checkbox'  value='agree'  {...register('agreeToTandC')}  />
				    </div>
				    <div>
					    <select  {...register('car')}>
						    <option  value='BMW'>BMW</option>
						    <option  value='Mercedies'>Mercedies</option>
						    <option  value='Audi'>Audi</option>
					    </select>
				    </div>
				    <div>
					    <input  type='submit'  value='submit form'/>
				    </div>
			    </form>
		    </div>
	    )
    }
    export  default  App;

***result of console.log(data)***

    {
	    age:  "25"
	    agreeToTandC:  "agree"
	    car:  "Mercedies"
	    email:  "vishalanand1033@gmail.com"
	    gender:  "male"
	    isAdult:  true
	    password:  "123"
	    userName:  "Anandvikas008"
    }
## Nested Data (object)

    <form  onSubmit={handleSubmit(submitFun)}>
	    ...
	    <div>
		    <input  type='text'  placeholder='State'  {...register('address.state')}  />
		    <input  type='text'  placeholder='City'  {...register('address.city')}  />
		    <input  type='text'  placeholder='Street'  {...register('address.street')}  />
	    </div>
	    <div>
		    <input  type='submit'  value='submit form'  />
	    </div>
    </form>
***result of console.log(data)***

    {
   	    age:  "25"
   	    agreeToTandC:  "agree"
   	    car:  "Mercedies"
   	    email:  "vishalanand1033@gmail.com"
   	    gender:  "male"
   	    isAdult:  true
   	    password:  "123"
   	    userName:  "Anandvikas008"
   	    address: {
	   	    city: "Jaipur"
	   	    state: "Rajasthan"
	   	    street: "sector17"
   	    }
    }
## Nested Data (array)
    <form  onSubmit={handleSubmit(submitFun)}>
	    ...
	    <div>
		    <input  type='text'  placeholder='State'  {...register[0]}  />
		    <input  type='text'  placeholder='City'  {...register[1]}  />
		    <input  type='text'  placeholder='Street'  {...register[2]}  />
	    </div>
	    <div>
		    <input  type='submit'  value='submit form'  />
	    </div>
    </form>
***result of console.log(data)***

    {
        age:  "25"
        agreeToTandC:  "agree"
        car:  "Mercedies"
        email:  "vishalanand1033@gmail.com"
        gender:  "male"
        isAdult:  true
        password:  "123"
        userName:  "Anandvikas008"
        address: ['Rajasthan', 'Jaipur', 'sector17']
    }