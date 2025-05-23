# NEXTJS - Ecommerce
> Build a E-commerce Full Stack with Nextjs + Mongodb + React Hook + Bootstrap 4
> Instructions on how to use nextjs in conjunction with mongodb to build a basic e-commerce website.




## Demo: https://nextjs-ecommerce-six.vercel.app/

================================================
FILE: README.md
================================================
# NEXTJS - Ecommerce
> Build a E-commerce Full Stack with Nextjs + Mongodb + React Hook + Bootstrap 4
> Instructions on how to use nextjs in conjunction with mongodb to build a basic e-commerce website.

## Demo: https://nextjs-ecommerce-six.vercel.app/
## Install dependencies 
### `npm install`

Directory structure:
└── praveenarjun-ecommerce-website/
    ├── README.md
    ├── next.config.js
    ├── package.json
    ├── components/
    │   ├── CartItem.js
    │   ├── Filter.js
    │   ├── Layout.js
    │   ├── Loading.js
    │   ├── Modal.js
    │   ├── NavBar.js
    │   ├── Notify.js
    │   ├── OrderDetail.js
    │   ├── paypalBtn.js
    │   ├── Toast.js
    │   └── product/
    │       └── ProductItem.js
    ├── middleware/
    │   └── auth.js
    ├── models/
    │   ├── categoriesModel.js
    │   ├── orderModel.js
    │   ├── productModel.js
    │   └── userModel.js
    ├── pages/
    │   ├── _app.js
    │   ├── _document.js
    │   ├── cart.js
    │   ├── categories.js
    │   ├── index.js
    │   ├── profile.js
    │   ├── register.js
    │   ├── signin.js
    │   ├── users.js
    │   ├── api/
    │   │   ├── auth/
    │   │   │   ├── accessToken.js
    │   │   │   ├── login.js
    │   │   │   └── register.js
    │   │   ├── categories/
    │   │   │   ├── [id].js
    │   │   │   └── index.js
    │   │   ├── order/
    │   │   │   ├── index.js
    │   │   │   ├── delivered/
    │   │   │   │   └── [id].js
    │   │   │   └── payment/
    │   │   │       └── [id].js
    │   │   ├── product/
    │   │   │   ├── [id].js
    │   │   │   └── index.js
    │   │   └── user/
    │   │       ├── [id].js
    │   │       ├── index.js
    │   │       └── resetPassword.js
    │   ├── create/
    │   │   └── [[...id]].js
    │   ├── edit_user/
    │   │   └── [id].js
    │   ├── order/
    │   │   └── [id].js
    │   └── product/
    │       └── [id].js
    ├── public/
    ├── store/
    │   ├── Actions.js
    │   ├── GlobalState.js
    │   └── Reducers.js
    ├── styles/
    │   ├── globals.css
    │   ├── loading.css
    │   ├── product.css
    │   ├── products_manager.css
    │   └── profile.css
    └── utils/
        ├── connectDB.js
        ├── fetchData.js
        ├── filterSearch.js
        ├── generateToken.js
        ├── imageUpload.js
        └── valid.js

## Connect to your mongodb and add info in next.config.js

## Run the project http://localhost:3000
### `npm run dev`


### User interface 

![alt](https://res.cloudinary.com/devat-channel/image/upload/v1610956436/nextjs_media/2_dgx2op.png)

![alt](https://res.cloudinary.com/devat-channel/image/upload/v1610956526/nextjs_media/3_dkmrq1.png)

### Admin interface 

![alt](https://res.cloudinary.com/devat-channel/image/upload/v1610956354/nextjs_media/Untitled_axeubb.png)



================================================
FILE: next.config.js
================================================
module.exports = {
    env: {
        "BASE_URL": "http://localhost:3000",
        "MONGODB_URL": "YOUR_MONGODB_URL",
        "ACCESS_TOKEN_SECRET": "YOUR_ACCESS_TOKEN_SECRET",
        "REFRESH_TOKEN_SECRET": "YOUR_REFRESH_TOKEN_SECRET",
        "PAYPAL_CLIENT_ID": "YOUR_PAYPAL_CLIENT_ID",
        "CLOUD_UPDATE_PRESET": "YOUR_CLOUD_UPDATE_PRESET",
        "CLOUD_NAME": "YOUR_CLOUD_NAME",
        "CLOUD_API": "YOUR_CLOUD_API"
    }
}


================================================
FILE: package.json
================================================
{
  "name": "nextjs-ecommerce",
  "version": "0.1.0",
  "private": true,
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  },
  "dependencies": {
    "bcrypt": "^5.0.0",
    "js-cookie": "^2.2.1",
    "jsonwebtoken": "^8.5.1",
    "mongoose": "^5.10.14",
    "next": "10.0.1",
    "react": "17.0.1",
    "react-dom": "17.0.1"
  }
}



================================================
FILE: components/CartItem.js
================================================
import Link from 'next/link'
import { decrease, increase } from '../store/Actions'

const CartItem = ({item, dispatch, cart}) => {
    return (
        <tr>
            <td style={{width: '100px', overflow: 'hidden'}}>
                <img src={item.images[0].url} alt={item.images[0].url}
                className="img-thumbnail w-100"
                style={{minWidth: '80px', height: '80px'}} />
            </td>

            <td style={{minWidth: '200px'}} className="w-50 align-middle" >
                <h5 className="text-capitalize text-secondary">
                    <Link href={`/product/${item._id}`}>
                        <a>{item.title}</a>
                    </Link>
                </h5>

                <h6 className="text-danger">${item.quantity * item.price}</h6>
                {
                    item.inStock > 0
                    ? <p className="mb-1 text-danger">In Stock: {item.inStock}</p>
                    : <p className="mb-1 text-danger">Out Stock</p>
                }
            </td>

            <td className="align-middle" style={{minWidth: '150px'}}>
                <button className="btn btn-outline-secondary"
                onClick={ () => dispatch(decrease(cart, item._id)) } 
                disabled={item.quantity === 1 ? true : false} > - </button>

                <span className="px-3">{item.quantity}</span>

                <button className="btn btn-outline-secondary"
                onClick={ () => dispatch(increase(cart, item._id)) }
                disabled={item.quantity === item.inStock ? true : false} > + </button>
            </td>

            <td className="align-middle" style={{minWidth: '50px', cursor: 'pointer'}}>
                <i className="far fa-trash-alt text-danger" aria-hidden="true" 
                style={{fontSize: '18px'}} data-toggle="modal" data-target="#exampleModal"
                onClick={() => dispatch({
                    type: 'ADD_MODAL',
                    payload: [{ data: cart, id: item._id, title: item.title, type: 'ADD_CART' }]
                })} ></i>
            </td>
        </tr>
    )
}

export default CartItem


================================================
FILE: components/Filter.js
================================================
import React, {useState, useEffect} from 'react'
import filterSearch from '../utils/filterSearch'
import {getData} from '../utils/fetchData'
import {useRouter} from 'next/router'

const Filter = ({state}) => {
    const [search, setSearch] = useState('')
    const [sort, setSort] = useState('')
    const [category, setCategory] = useState('')

    const {categories} = state

    const router = useRouter()


    const handleCategory = (e) => {
        setCategory(e.target.value)
        filterSearch({router, category: e.target.value})
    }

    const handleSort = (e) => {
        setSort(e.target.value)
        filterSearch({router, sort: e.target.value})
    }

    useEffect(() => {
        filterSearch({router, search: search ? search.toLowerCase() : 'all'})
    },[search])

    return (
        <div className="input-group">
            <div className="input-group-prepend col-md-2 px-0 mt-2">
                <select className="custom-select text-capitalize"
                value={category} onChange={handleCategory}>

                    <option value="all">All Products</option>

                    {
                        categories.map(item => (
                            <option key={item._id} value={item._id}>{item.name}</option>
                        ))
                    }
                </select>
            </div>

            <form autoComplete="off" className="mt-2 col-md-8 px-0">
                <input type="text" className="form-control" list="title_product"
                value={search.toLowerCase()} onChange={e => setSearch(e.target.value)} />
            </form>

            <div className="input-group-prepend col-md-2 px-0 mt-2">
                <select className="custom-select text-capitalize"
                value={sort} onChange={handleSort}>

                     <option value="-createdAt">Newest</option>
                     <option value="oldest">Oldest</option>
                     <option value="-sold">Best sales</option>
                     <option value="-price">Price: Hight-Low</option>
                     <option value="price">Price: Low-Hight</option>

                </select>
            </div>


        </div>
    )
}

export default Filter



================================================
FILE: components/Layout.js
================================================
import React from 'react'
import NavBar from './NavBar'
import Notify from './Notify'
import Modal from './Modal'

function Layout({children}) {
    return (
        <div className="container">
            <NavBar />
            <Notify />
            <Modal />
            {children}
        </div>
    )
}

export default Layout



================================================
FILE: components/Loading.js
================================================
const Loading = () => {
    return (
        <div className="position-fixed w-100 h-100 text-center loading"
        style={{background: '#0008', color: 'white', top: 0, left: 0, zIndex: 9}}>
            <svg width="205" height="250" viewBox="0 0 40 50">
                <polygon strokeWidth="1" stroke="#fff" fill="none"
                points="20,1 40,40 1,40"></polygon>
                <text fill="#fff" x="5" y="47">Loading</text>
            </svg>
        </div>
    )
}

export default Loading


================================================
FILE: components/Modal.js
================================================
import { useContext } from 'react'
import { DataContext } from '../store/GlobalState'
import { deleteItem } from '../store/Actions'
import { deleteData } from '../utils/fetchData'
import {useRouter} from 'next/router'


const Modal = () => {
    const {state, dispatch} = useContext(DataContext)
    const { modal, auth } = state

    const router = useRouter()

    const deleteUser = (item) => {
        dispatch(deleteItem(item.data, item.id, item.type))
        
        deleteData(`user/${item.id}`, auth.token)
        .then(res => {
            if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})
            return dispatch({type: 'NOTIFY', payload: {success: res.msg}})
        })
    }

    const deleteCategories = (item) => {
        deleteData(`categories/${item.id}`, auth.token)
        .then(res => {
            if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})

            dispatch(deleteItem(item.data, item.id, item.type))
            return dispatch({type: 'NOTIFY', payload: {success: res.msg}})
        })
    }

    const deleteProduct = (item) => {
        dispatch({type: 'NOTIFY', payload: {loading: true}})
        deleteData(`product/${item.id}`, auth.token)
        .then(res => {
            if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})
            dispatch({type: 'NOTIFY', payload: {success: res.msg}})
            return router.push('/')
        })
    }

    const handleSubmit = () => {
        if(modal.length !== 0){
            for(const item of modal){
                if(item.type === 'ADD_CART'){
                    dispatch(deleteItem(item.data, item.id, item.type))
                }

                if(item.type === 'ADD_USERS') deleteUser(item)
        
                if(item.type === 'ADD_CATEGORIES') deleteCategories(item)
        
                if(item.type === 'DELETE_PRODUCT') deleteProduct(item)
        
                dispatch({ type: 'ADD_MODAL', payload: [] })
            }
        }
    }

    return(
        <div className="modal fade" id="exampleModal" tabIndex="-1" role="dialog" aria-labelledby="exampleModalLabel" aria-hidden="true">
            <div className="modal-dialog" role="document">
                <div className="modal-content">
                <div className="modal-header">
                    <h5 className="modal-title text-capitalize" id="exampleModalLabel">
                        {modal.length !== 0 && modal[0].title}
                    </h5>
                    <button type="button" className="close" data-dismiss="modal" aria-label="Close">
                    <span aria-hidden="true">&times;</span>
                    </button>
                </div>
                <div className="modal-body">
                    Do you want to delete this item?
                </div>
                <div className="modal-footer">
                    <button type="button" className="btn btn-secondary" data-dismiss="modal" onClick={handleSubmit}>Yes</button>
                    <button type="button" className="btn btn-primary" data-dismiss="modal">Cancel</button>
                </div>
                </div>
            </div>
        </div>
    )
}

export default Modal


================================================
FILE: components/NavBar.js
================================================
import React, { useContext } from 'react'
import Link from 'next/link'
import {useRouter} from 'next/router'
import {DataContext} from '../store/GlobalState'
import Cookie from 'js-cookie'

function NavBar() {
    const router = useRouter()
    const {state, dispatch} = useContext(DataContext)
    const { auth, cart } = state


    const isActive = (r) => {
        if(r === router.pathname){
            return " active"
        }else{
            return ""
        }
    }

    const handleLogout = () => {
        Cookie.remove('refreshtoken', {path: 'api/auth/accessToken'})
        localStorage.removeItem('firstLogin')
        dispatch({ type: 'AUTH', payload: {} })
        dispatch({ type: 'NOTIFY', payload: {success: 'Logged out!'} })
        return router.push('/')
    }

    const adminRouter = () => {
        return(
            <>
            <Link href="/users">
                <a className="dropdown-item">Users</a>
            </Link>
            <Link href="/create">
                <a className="dropdown-item">Products</a>
            </Link>
            <Link href="/categories">
                <a className="dropdown-item">Categories</a>
            </Link>
            </>
        )
    }

    const loggedRouter = () => {
        return(
            <li className="nav-item dropdown">
                <a className="nav-link dropdown-toggle" href="#" id="navbarDropdownMenuLink" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
                    <img src={auth.user.avatar} alt={auth.user.avatar} 
                    style={{
                        borderRadius: '50%', width: '30px', height: '30px',
                        transform: 'translateY(-3px)', marginRight: '3px'
                    }} /> {auth.user.name}
                </a>

                <div className="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
                    <Link href="/profile">
                        <a className="dropdown-item">Profile</a>
                    </Link>
                    {
                        auth.user.role === 'admin' && adminRouter()
                    }
                    <div className="dropdown-divider"></div>
                    <button className="dropdown-item" onClick={handleLogout}>Logout</button>
                </div>
            </li>
        )
    }

    return (
        <nav className="navbar navbar-expand-lg navbar-light bg-light">
            <Link  href="/">
                <a className="navbar-brand">DEVAT</a>
            </Link>
            <button className="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavDropdown" aria-controls="navbarNavDropdown" aria-expanded="false" aria-label="Toggle navigation">
                <span className="navbar-toggler-icon"></span>
            </button>
            <div className="collapse navbar-collapse justify-content-end" id="navbarNavDropdown">
                <ul className="navbar-nav p-1">
                    <li className="nav-item">
                        <Link href="/cart">
                            <a className={"nav-link" + isActive('/cart')}>
                                <i className="fas fa-shopping-cart position-relative" aria-hidden="true">
                                    <span className="position-absolute"
                                    style={{
                                        padding: '3px 6px',
                                        background: '#ed143dc2',
                                        borderRadius: '50%',
                                        top: '-10px',
                                        right: '-10px',
                                        color: 'white',
                                        fontSize: '14px'
                                    }}>
                                        {cart.length}
                                    </span>
                                </i> Cart
                            </a>
                        </Link>
                    </li>
                    {
                        Object.keys(auth).length === 0 
                        ? <li className="nav-item">
                            <Link href="/signin">
                                <a className={"nav-link" + isActive('/signin')}>
                                    <i className="fas fa-user" aria-hidden="true"></i> Sign in
                                </a>
                            </Link>
                        </li>
                        : loggedRouter()
                    }
                </ul>
            </div>
        </nav>
    )
}

export default NavBar



================================================
FILE: components/Notify.js
================================================
import {useContext} from 'react'
import {DataContext} from '../store/GlobalState'
import Loading from './Loading'
import Toast from './Toast'

const Notify = () => {
    const {state, dispatch} = useContext(DataContext)
    const { notify } = state

    return(
        <> 
            {notify.loading && <Loading />}
            {notify.error && 
                <Toast
                    msg={{ msg: notify.error, title: "Error" }}
                    handleShow={() => dispatch({ type: 'NOTIFY', payload: {} })}
                    bgColor="bg-danger"
                />
            }

            {notify.success && 
                <Toast
                    msg={{ msg: notify.success, title: "Success" }}
                    handleShow={() => dispatch({ type: 'NOTIFY', payload: {} })}
                    bgColor="bg-success"
                />
            }
        </>
    )
}


export default Notify



================================================
FILE: components/OrderDetail.js
================================================
import Link from 'next/link'
import PaypalBtn from './paypalBtn'
import {patchData} from '../utils/fetchData'
import {updateItem} from '../store/Actions'

const OrderDetail = ({orderDetail, state, dispatch}) => {
    const {auth, orders} = state

    const handleDelivered = (order) => {
        dispatch({type: 'NOTIFY', payload: {loading: true}})

        patchData(`order/delivered/${order._id}`, null, auth.token)
        .then(res => {
            if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})

            const { paid, dateOfPayment, method, delivered } = res.result

            dispatch(updateItem(orders, order._id, {
                ...order, paid, dateOfPayment, method, delivered
            }, 'ADD_ORDERS'))

            return dispatch({type: 'NOTIFY', payload: {success: res.msg}})
        })
    }

    if(!auth.user) return null;
    return(
        <>
        {
            orderDetail.map(order => (
            <div key={order._id} style={{margin: '20px auto'}} className="row justify-content-around">

                <div className="text-uppercase my-3" style={{maxWidth: '600px'}}>
                    <h2 className="text-break">Order {order._id}</h2>

                    <div className="mt-4 text-secondary">
                        <h3>Shipping</h3>
                        <p>Name: {order.user.name}</p>
                        <p>Email: {order.user.email}</p>
                        <p>Address: {order.address}</p>
                        <p>Mobile: {order.mobile}</p>

                        <div className={`alert ${order.delivered ? 'alert-success' : 'alert-danger'}
                        d-flex justify-content-between align-items-center`} role="alert">
                            {
                                order.delivered ? `Deliverd on ${order.updatedAt}` : 'Not Delivered'
                            }
                            {
                                auth.user.role === 'admin' && !order.delivered &&
                                <button className="btn btn-dark text-uppercase"
                                onClick={() => handleDelivered(order)}>
                                    Mark as delivered
                                </button>
                            }
                            
                        </div>

                        <h3>Payment</h3>
                        {
                            order.method && <h6>Method: <em>{order.method}</em></h6>
                        }
                        
                        {
                            order.paymentId && <p>PaymentId: <em>{order.paymentId}</em></p>
                        }
                        
                        <div className={`alert ${order.paid ? 'alert-success' : 'alert-danger'}
                        d-flex justify-content-between align-items-center`} role="alert">
                            {
                                order.paid ? `Paid on ${order.dateOfPayment}` : 'Not Paid'
                            }
                            
                        </div>

                        <div>
                            <h3>Order Items</h3>
                            {
                                order.cart.map(item => (
                                    <div className="row border-bottom mx-0 p-2 justify-content-betwenn
                                    align-items-center" key={item._id} style={{maxWidth: '550px'}}>
                                        <img src={item.images[0].url} alt={item.images[0].url}
                                        style={{width: '50px', height: '45px', objectFit: 'cover'}} />

                                        <h5 className="flex-fill text-secondary px-3 m-0">
                                            <Link href={`/product/${item._id}`}>
                                                <a>{item.title}</a>
                                            </Link>
                                        </h5>

                                        <span className="text-info m-0">
                                            {item.quantity} x ${item.price} = ${item.price * item.quantity}
                                        </span>

                                    </div>
                                ))
                            }
                        </div>

                    </div>

                </div>
                            
                {
                    !order.paid && auth.user.role !== 'admin' &&
                    <div className="p-4">
                        <h2 className="mb-4 text-uppercase">Total: ${order.total}</h2>
                        <PaypalBtn order={order} />
                    </div>
                }
               
            </div>
            ))
        }
        </>
    )
}

export default OrderDetail


================================================
FILE: components/paypalBtn.js
================================================
import { useEffect, useRef, useContext } from 'react'
import { patchData } from '../utils/fetchData'
import {DataContext} from '../store/GlobalState'
import {updateItem} from '../store/Actions'

const paypalBtn = ({order}) => {
    const refPaypalBtn = useRef()
    const {state, dispatch} = useContext(DataContext)
    const { auth, orders} = state

    useEffect(() => {
        paypal.Buttons({
            createOrder: function(data, actions) {
              // This function sets up the details of the transaction, including the amount and line item details.
              return actions.order.create({
                purchase_units: [{
                  amount: {
                    value: order.total
                  }
                }]
              });
            },
            onApprove: function(data, actions) {
              // This function captures the funds from the transaction.
              dispatch({ type: 'NOTIFY', payload: {loading: true} })

              return actions.order.capture().then(function(details) {

                patchData(`order/payment/${order._id}`, {
                  paymentId: details.payer.payer_id
                }, auth.token)
                .then(res => {
                  if(res.err) return dispatch({ type: 'NOTIFY', payload: {error: res.err} })
                  
                  dispatch(updateItem(orders, order._id, {
                    ...order, 
                    paid: true, dateOfPayment: details.create_time,
                    paymentId: details.payer.payer_id, method: 'Paypal'
                  }, 'ADD_ORDERS'))

                  return dispatch({ type: 'NOTIFY', payload: {success: res.msg} })
                })
                // This function shows a transaction success message to your buyer.
              });
            }
        }).render(refPaypalBtn.current);
    },[])

    return(
        <div ref={refPaypalBtn}></div>
    )
}

export default paypalBtn


================================================
FILE: components/Toast.js
================================================
const Toast = ({msg, handleShow, bgColor}) => {
    return(
        <div className={`toast show position-fixed text-light ${bgColor}`}
        style={{ top: '5px', right: '5px', zIndex: 9, minWidth: '280px' }} >

            <div className={`toast-header ${bgColor} text-light`}>
                <strong className="mr-auto text-light">{msg.title}</strong>

                <button type="button" className="ml-2 mb-1 close text-light" 
                data-dismiss="toast" style={{ outline: 'none'}} 
                onClick={handleShow}>x</button>
            </div>

            <div className="toast-body">{msg.msg}</div>

        </div>
    )
}

export default Toast


================================================
FILE: components/product/ProductItem.js
================================================
import Link from 'next/link'
import { useContext } from 'react'
import { DataContext } from '../../store/GlobalState'
import { addToCart } from '../../store/Actions'

const ProductItem = ({product, handleCheck}) => {
    const { state, dispatch } = useContext(DataContext)
    const { cart, auth } = state

    const userLink = () => {
        return(
            <>
                <Link href={`product/${product._id}`}>
                    <a className="btn btn-info"
                    style={{marginRight: '5px', flex: 1}}>View</a>
                </Link>
                <button className="btn btn-success"
                style={{marginLeft: '5px', flex: 1}}
                disabled={product.inStock === 0 ? true : false} 
                onClick={() => dispatch(addToCart(product, cart))} >
                    Buy
                </button>
            </>
        )
    }

    const adminLink = () => {
        return(
            <>
                <Link href={`create/${product._id}`}>
                    <a className="btn btn-info"
                    style={{marginRight: '5px', flex: 1}}>Edit</a>
                </Link>
                <button className="btn btn-danger"
                style={{marginLeft: '5px', flex: 1}}
                data-toggle="modal" data-target="#exampleModal"
                onClick={() => dispatch({
                    type: 'ADD_MODAL',
                    payload: [{ 
                        data: '', id: product._id, 
                        title: product.title, type: 'DELETE_PRODUCT' 
                    }]
                })} >
                    Delete
                </button>
            </>
        )
    }

    return(
        <div className="card" style={{ width: '18rem' }}>
            {
                auth.user && auth.user.role === 'admin' &&
                <input type="checkbox" checked={product.checked}
                className="position-absolute"
                style={{height: '20px', width: '20px'}}
                onChange={() => handleCheck(product._id)} />
            }
            <img className="card-img-top" src={product.images[0].url} alt={product.images[0].url} />
            <div className="card-body">
                <h5 className="card-title text-capitalize" title={product.title}>
                    {product.title}
                </h5>

                <div className="row justify-content-between mx-0">
                    <h6 className="text-danger">${product.price}</h6>
                    {
                        product.inStock > 0
                        ? <h6 className="text-danger">In Stock: {product.inStock}</h6>
                        : <h6 className="text-danger">Out Stock</h6>
                    }
                </div>

                <p className="card-text" title={product.description}>
                    {product.description}
                </p>
                    
                <div className="row justify-content-between mx-0">
                    {!auth.user || auth.user.role !== "admin" ? userLink() : adminLink()}
                </div>
            </div>
        </div>
    )
}


export default ProductItem


================================================
FILE: middleware/auth.js
================================================
import jwt from 'jsonwebtoken'
import Users from '../models/userModel'


const auth = async (req, res) => {
    const token = req.headers.authorization;
    if(!token) return res.status(400).json({err: 'Invalid Authentication.'})

    const decoded = jwt.verify(token, process.env.ACCESS_TOKEN_SECRET)
    if(!decoded) return res.status(400).json({err: 'Invalid Authentication.'})

    const user = await Users.findOne({_id: decoded.id})

    return {id: user._id, role: user.role, root: user.root};
}


export default auth


================================================
FILE: models/categoriesModel.js
================================================
import mongoose from 'mongoose'

const CategoriesSchema = new mongoose.Schema({
    name: {
        type: String,
        required: true,
        trim: true
    }
}, {
    timestamps: true
})

let Dataset = mongoose.models.categories || mongoose.model('categories', CategoriesSchema)
export default Dataset


================================================
FILE: models/orderModel.js
================================================
import mongoose from 'mongoose'

const orderSchema = new mongoose.Schema({
    user: {
        type: mongoose.Types.ObjectId,
        ref: 'user'
    },
    address: String,
    mobile: String,
    cart: Array,
    total: Number,
    paymentId: String,
    method: String,
    delivered: {
        type: Boolean,
        default: false
    },
    paid: {
        type: Boolean,
        default: false
    },
    dateOfPayment: Date
}, {
    timestamps: true
})

let Dataset = mongoose.models.order || mongoose.model('order', orderSchema)
export default Dataset


================================================
FILE: models/productModel.js
================================================
import mongoose from 'mongoose'

const productSchema = new mongoose.Schema({
    title: {
        type: String,
        required: true,
        trim: true
    },
    price: {
        type: Number,
        required: true,
        trim: true
    },
    description: {
        type: String,
        required: true
    },
    content: {
        type: String,
        required: true
    },
    images: {
        type: Array,
        required: true
    },
    category: {
        type: String,
        required: true
    },
    checked: {
        type: Boolean,
        default: false
    },
    inStock: {
        type: Number,
        default: 0
    },
    sold: {
        type: Number,
        default: 0
    }
}, {
    timestamps: true
})

let Dataset = mongoose.models.product || mongoose.model('product', productSchema)
export default Dataset


================================================
FILE: models/userModel.js
================================================
import mongoose from 'mongoose'

const userSchema = new mongoose.Schema({
    name: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true,
        unique: true
    },
    password: {
        type: String,
        required: true
    },
    role: {
        type: String,
        default: 'user'
    },
    root: {
        type: Boolean,
        default: false
    },
    avatar: {
        type: String,
        default: 'https://res.cloudinary.com/devatchannel/image/upload/v1602752402/avatar/avatar_cugq40.png'
    }
}, {
    timestamps: true
})

let Dataset = mongoose.models.user || mongoose.model('user', userSchema)
export default Dataset


================================================
FILE: pages/_app.js
================================================
import '../styles/globals.css'
import Layout from '../components/Layout'
import { DataProvider } from '../store/GlobalState'

function MyApp({ Component, pageProps }) {
  return (
    <DataProvider>
      <Layout>
        <Component {...pageProps} />
      </Layout>
    </DataProvider>
  )
}

export default MyApp



================================================
FILE: pages/_document.js
================================================
import Document, {Html, Head, Main, NextScript} from 'next/document'

class MyDocument extends Document{
    render(){
        return(
            <Html lang="en">
                <Head>
                    <meta name="description" content="Dev AT E-commerce website with Next.js"/>
                    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" />
                    <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
                    <script src="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/js/bootstrap.bundle.min.js"></script>
                    <script src="https://kit.fontawesome.com/a076d05399.js"></script>
                    <script src={`https://www.paypal.com/sdk/js?client-id=${process.env.PAYPAL_CLIENT_ID}`}></script>
                </Head>
                <body>
                    <Main />
                    <NextScript />
                </body>
            </Html>
        )
    }
}

export default MyDocument


================================================
FILE: pages/cart.js
================================================
import Head from 'next/head'
import { useContext, useState, useEffect } from 'react'
import { DataContext } from '../store/GlobalState'
import CartItem from '../components/CartItem'
import Link from 'next/link'
import { getData, postData } from '../utils/fetchData'
import { useRouter } from 'next/router'


const Cart = () => {
  const { state, dispatch } = useContext(DataContext)
  const { cart, auth, orders } = state

  const [total, setTotal] = useState(0)

  const [address, setAddress] = useState('')
  const [mobile, setMobile] = useState('')

  const [callback, setCallback] = useState(false)
  const router = useRouter()

  useEffect(() => {
    const getTotal = () => {
      const res = cart.reduce((prev, item) => {
        return prev + (item.price * item.quantity)
      },0)

      setTotal(res)
    }

    getTotal()
  },[cart])

  useEffect(() => {
    const cartLocal = JSON.parse(localStorage.getItem('__next__cart01__devat'))
    if(cartLocal && cartLocal.length > 0){
      let newArr = []
      const updateCart = async () => {
        for (const item of cartLocal){
          const res = await getData(`product/${item._id}`)
          const { _id, title, images, price, inStock, sold } = res.product
          if(inStock > 0){
            newArr.push({ 
              _id, title, images, price, inStock, sold,
              quantity: item.quantity > inStock ? 1 : item.quantity
            })
          }
        }

        dispatch({ type: 'ADD_CART', payload: newArr })
      }

      updateCart()
    } 
  },[callback])

  const handlePayment = async () => {
    if(!address || !mobile)
    return dispatch({ type: 'NOTIFY', payload: {error: 'Please add your address and mobile.'}})

    let newCart = [];
    for(const item of cart){
      const res = await getData(`product/${item._id}`)
      if(res.product.inStock - item.quantity >= 0){
        newCart.push(item)
      }
    }
    
    if(newCart.length < cart.length){
      setCallback(!callback)
      return dispatch({ type: 'NOTIFY', payload: {
        error: 'The product is out of stock or the quantity is insufficient.'
      }})
    }

    dispatch({ type: 'NOTIFY', payload: {loading: true} })

    postData('order', {address, mobile, cart, total}, auth.token)
    .then(res => {
      if(res.err) return dispatch({ type: 'NOTIFY', payload: {error: res.err} })

      dispatch({ type: 'ADD_CART', payload: [] })
      
      const newOrder = {
        ...res.newOrder,
        user: auth.user
      }
      dispatch({ type: 'ADD_ORDERS', payload: [...orders, newOrder] })
      dispatch({ type: 'NOTIFY', payload: {success: res.msg} })
      return router.push(`/order/${res.newOrder._id}`)
    })

  }
  
  if( cart.length === 0 ) 
    return <img className="img-responsive w-100" src="/empty_cart.jpg" alt="not empty"/>

    return(
      <div className="row mx-auto">
        <Head>
          <title>Cart Page</title>
        </Head>

        <div className="col-md-8 text-secondary table-responsive my-3">
          <h2 className="text-uppercase">Shopping Cart</h2>

          <table className="table my-3">
            <tbody>
              {
                cart.map(item => (
                  <CartItem key={item._id} item={item} dispatch={dispatch} cart={cart} />
                ))
              }
            </tbody>
          </table>
        </div>

        <div className="col-md-4 my-3 text-right text-uppercase text-secondary">
            <form>
              <h2>Shipping</h2>

              <label htmlFor="address">Address</label>
              <input type="text" name="address" id="address"
              className="form-control mb-2" value={address}
              onChange={e => setAddress(e.target.value)} />

              <label htmlFor="mobile">Mobile</label>
              <input type="text" name="mobile" id="mobile"
              className="form-control mb-2" value={mobile}
              onChange={e => setMobile(e.target.value)} />
            </form>

            <h3>Total: <span className="text-danger">${total}</span></h3>

            
            <Link href={auth.user ? '#!' : '/signin'}>
              <a className="btn btn-dark my-2" onClick={handlePayment}>Proceed with payment</a>
            </Link>
            
        </div>
      </div>
    )
  }
  
export default Cart


================================================
FILE: pages/categories.js
================================================
import Head from 'next/head'
import {useContext, useState} from 'react'
import {DataContext} from '../store/GlobalState'
import {updateItem} from '../store/Actions'
import { postData, putData } from "../utils/fetchData";

const Categories = () => {
    const [name, setName] = useState('')

    const {state, dispatch} = useContext(DataContext)
    const {categories, auth} = state
    
    const [id, setId] = useState('')

    const createCategory = async () => {
        if(auth.user.role !== 'admin')
        return dispatch({type: 'NOTIFY', payload: {error: 'Authentication is not vaild.'}})

        if(!name) return dispatch({type: 'NOTIFY', payload: {error: 'Name can not be left blank.'}})

        dispatch({type: 'NOTIFY', payload: {loading: true}})

        let res;
        if(id){
            res = await putData(`categories/${id}`, {name}, auth.token)
            if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})
            dispatch(updateItem(categories, id, res.category, 'ADD_CATEGORIES'))

        }else{
            res = await postData('categories', {name}, auth.token)
            if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})
            dispatch({type: "ADD_CATEGORIES", payload: [...categories, res.newCategory]})
        }
        setName('')
        setId('')
        return dispatch({type: 'NOTIFY', payload: {success: res.msg}})
    }

    const handleEditCategory = (catogory) => {
        setId(catogory._id)
        setName(catogory.name)
    }

    return(
        <div className="col-md-6 mx-auto my-3">
            <Head>
                <title>Categories</title>
            </Head>

            <div className="input-group mb-3">
                <input type="text" className="form-control"
                placeholder="Add a new category" value={name}
                onChange={e => setName(e.target.value)} />

                <button className="btn btn-secondary ml-1"
                onClick={createCategory}>
                    {id ? "Update": "Create"}
                </button>
            </div>

            {
                categories.map(catogory => (
                    <div key={catogory._id} className="card my-2 text-capitalize">
                        <div className="card-body d-flex justify-content-between">
                            {catogory.name}

                            <div style={{cursor: 'pointer'}}>
                                <i className="fas fa-edit mr-2 text-info"
                                onClick={() => handleEditCategory(catogory)}></i>

                                <i className="fas fa-trash-alt text-danger"
                                data-toggle="modal" data-target="#exampleModal"
                                onClick={() => dispatch({
                                    type: 'ADD_MODAL',
                                    payload: [{ 
                                        data: categories, id: catogory._id, 
                                        title: catogory.name, type: 'ADD_CATEGORIES' 
                                    }]
                                })} ></i>
                            </div>

                        </div>
                    </div>
                ))
            }
           
        </div>
    )
}

export default Categories


================================================
FILE: pages/index.js
================================================
import Head from 'next/head'
import { useState, useContext, useEffect } from 'react'
import {DataContext} from '../store/GlobalState'

import { getData } from '../utils/fetchData'
import ProductItem from '../components/product/ProductItem'
import filterSearch from '../utils/filterSearch'
import {useRouter} from 'next/router'
import Filter from '../components/Filter'

const Home = (props) => {
  const [products, setProducts] = useState(props.products)

  const [isCheck, setIsCheck] = useState(false)
  const [page, setPage] = useState(1)
  const router = useRouter()

  const {state, dispatch} = useContext(DataContext)
  const {auth} = state

  useEffect(() => {
    setProducts(props.products)
  },[props.products])

  useEffect(() => {
    if(Object.keys(router.query).length === 0) setPage(1)
  },[router.query])

  const handleCheck = (id) => {
    products.forEach(product => {
      if(product._id === id) product.checked = !product.checked
    })
    setProducts([...products])
  }

  const handleCheckALL = () => {
    products.forEach(product => product.checked = !isCheck)
    setProducts([...products])
    setIsCheck(!isCheck)
  }

  const handleDeleteAll = () => {
    let deleteArr = [];
    products.forEach(product => {
      if(product.checked){
          deleteArr.push({
            data: '', 
            id: product._id, 
            title: 'Delete all selected products?', 
            type: 'DELETE_PRODUCT'
          })
      }
    })

    dispatch({type: 'ADD_MODAL', payload: deleteArr})
  }

  const handleLoadmore = () => {
    setPage(page + 1)
    filterSearch({router, page: page + 1})
  }

  return(
    <div className="home_page">
      <Head>
        <title>Home Page</title>
      </Head>

      <Filter state={state} />

      {
        auth.user && auth.user.role === 'admin' &&
        <div className="delete_all btn btn-danger mt-2" style={{marginBottom: '-10px'}}>
          <input type="checkbox" checked={isCheck} onChange={handleCheckALL}
          style={{width: '25px', height: '25px', transform: 'translateY(8px)'}} />

          <button className="btn btn-danger ml-2"
          data-toggle="modal" data-target="#exampleModal"
          onClick={handleDeleteAll}>
            DELETE ALL
          </button>
        </div>
      }

      <div className="products">
        {
          products.length === 0 
          ? <h2>No Products</h2>

          : products.map(product => (
            <ProductItem key={product._id} product={product} handleCheck={handleCheck} />
          ))
        }
      </div>
      
      {
        props.result < page * 6 ? ""
        : <button className="btn btn-outline-info d-block mx-auto mb-4"
        onClick={handleLoadmore}>
          Load more
        </button>
      }
    
    </div>
  )
}


export async function getServerSideProps({query}) {
  const page = query.page || 1
  const category = query.category || 'all'
  const sort = query.sort || ''
  const search = query.search || 'all'

  const res = await getData(
    `product?limit=${page * 6}&category=${category}&sort=${sort}&title=${search}`
  )
  // server side rendering
  return {
    props: {
      products: res.products,
      result: res.result
    }, // will be passed to the page component as props
  }
}

export default Home


================================================
FILE: pages/profile.js
================================================
import Head from 'next/head'
import { useState, useContext, useEffect } from 'react'
import { DataContext } from '../store/GlobalState'
import Link from 'next/link'

import valid from '../utils/valid'
import { patchData } from '../utils/fetchData'

import {imageUpload} from '../utils/imageUpload'

const Profile = () => {
    const initialSate = {
        avatar: '',
        name: '',
        password: '',
        cf_password: ''
    }
    const [data, setData] = useState(initialSate)
    const { avatar, name, password, cf_password } = data

    const {state, dispatch} = useContext(DataContext)
    const { auth, notify, orders } = state

    useEffect(() => {
        if(auth.user) setData({...data, name: auth.user.name})
    },[auth.user])

    const handleChange = (e) => {
        const { name, value } = e.target
        setData({...data, [name]:value})
        dispatch({ type: 'NOTIFY', payload: {} })
    }

    const handleUpdateProfile = e => {
        e.preventDefault()
        if(password){
            const errMsg = valid(name, auth.user.email, password, cf_password)
            if(errMsg) return dispatch({ type: 'NOTIFY', payload: {error: errMsg} })
            updatePassword()
        }

        if(name !== auth.user.name || avatar) updateInfor()
    }

    const updatePassword = () => {
        dispatch({ type: 'NOTIFY', payload: {loading: true} })
        patchData('user/resetPassword', {password}, auth.token)
        .then(res => {
            if(res.err) return dispatch({ type: 'NOTIFY', payload: {error: res.err} })
            return dispatch({ type: 'NOTIFY', payload: {success: res.msg} })
        })
    }

    const changeAvatar = (e) => {
        const file = e.target.files[0]
        if(!file)
            return dispatch({type: 'NOTIFY', payload: {error: 'File does not exist.'}})

        if(file.size > 1024 * 1024) //1mb
            return dispatch({type: 'NOTIFY', payload: {error: 'The largest image size is 1mb.'}})

        if(file.type !== "image/jpeg" && file.type !== "image/png") //1mb
            return dispatch({type: 'NOTIFY', payload: {error: 'Image format is incorrect.'}})
        
        setData({...data, avatar: file})
    }

    const updateInfor = async () => {
        let media;
        dispatch({type: 'NOTIFY', payload: {loading: true}})

        if(avatar) media = await imageUpload([avatar])

        patchData('user', {
            name, avatar: avatar ? media[0].url : auth.user.avatar
        }, auth.token).then(res => {
            if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})

            dispatch({type: 'AUTH', payload: {
                token: auth.token,
                user: res.user
            }})
            return dispatch({type: 'NOTIFY', payload: {success: res.msg}})
        })
    }


    if(!auth.user) return null;
    return( 
        <div className="profile_page">
            <Head>
                <title>Profile</title>
            </Head>

            <section className="row text-secondary my-3">
                <div className="col-md-4">
                    <h3 className="text-center text-uppercase">
                        {auth.user.role === 'user' ? 'User Profile' : 'Admin Profile'}
                    </h3>

                    <div className="avatar">
                        <img src={avatar ? URL.createObjectURL(avatar) : auth.user.avatar} 
                        alt="avatar" />
                        <span>
                            <i className="fas fa-camera"></i>
                            <p>Change</p>
                            <input type="file" name="file" id="file_up"
                            accept="image/*" onChange={changeAvatar} />
                        </span>
                    </div>

                    <div className="form-group">
                        <label htmlFor="name">Name</label>
                        <input type="text" name="name" value={name} className="form-control"
                        placeholder="Your name" onChange={handleChange} />
                    </div>

                    <div className="form-group">
                        <label htmlFor="email">Email</label>
                        <input type="text" name="email" defaultValue={auth.user.email} 
                        className="form-control" disabled={true} />
                    </div>

                    <div className="form-group">
                        <label htmlFor="password">New Password</label>
                        <input type="password" name="password" value={password} className="form-control"
                        placeholder="Your new password" onChange={handleChange} />
                    </div>

                    <div className="form-group">
                        <label htmlFor="cf_password">Confirm New Password</label>
                        <input type="password" name="cf_password" value={cf_password} className="form-control"
                        placeholder="Confirm new password" onChange={handleChange} />
                    </div>

                    <button className="btn btn-info" disabled={notify.loading}
                    onClick={handleUpdateProfile}>
                        Update
                    </button>
                </div>

                <div className="col-md-8">
                    <h3 className="text-uppercase">Orders</h3>

                    <div className="my-3 table-responsive">
                        <table className="table-bordered table-hover w-100 text-uppercase"
                        style={{minWidth: '600px', cursor: 'pointer'}}>
                            <thead className="bg-light font-weight-bold">
                                <tr>
                                    <td className="p-2">id</td>
                                    <td className="p-2">date</td>
                                    <td className="p-2">total</td>
                                    <td className="p-2">delivered</td>
                                    <td className="p-2">paid</td>
                                </tr>
                            </thead>

                            <tbody>
                                {
                                    orders.map(order => (
                                        <tr key={order._id}>
                                            <td className="p-2">
                                                <Link href={`/order/${order._id}`}>
                                                    <a>{order._id}</a>
                                                </Link>
                                                
                                            </td>
                                            <td className="p-2">
                                                {new Date(order.createdAt).toLocaleDateString()}
                                            </td>
                                            <td className="p-2">${order.total}</td>
                                            <td className="p-2">
                                                {
                                                    order.delivered
                                                    ? <i className="fas fa-check text-success"></i>
                                                    : <i className="fas fa-times text-danger"></i>
                                                }
                                            </td>
                                            <td className="p-2">
                                                {
                                                    order.paid
                                                    ? <i className="fas fa-check text-success"></i>
                                                    : <i className="fas fa-times text-danger"></i>
                                                }
                                            </td>
                                        </tr> 
                                    ))
                                }
                            </tbody>

                        </table>
                    </div>
                </div>
            </section>
        </div>
    )
}

export default Profile


================================================
FILE: pages/register.js
================================================
import Head from 'next/head'
import Link from 'next/link'
import {useState, useContext, useEffect} from 'react'
import valid from '../utils/valid'
import {DataContext} from '../store/GlobalState'
import {postData} from '../utils/fetchData'
import { useRouter } from 'next/router'


const Register = () => {
  const initialState = { name: '', email: '', password: '', cf_password: '' }
  const [userData, setUserData] = useState(initialState)
  const { name, email, password, cf_password } = userData

  const {state, dispatch} = useContext(DataContext)
  const { auth } = state

  const router = useRouter()

  const handleChangeInput = e => {
    const {name, value} = e.target
    setUserData({...userData, [name]:value})
    dispatch({ type: 'NOTIFY', payload: {} })
  }

  const handleSubmit = async e => {
    e.preventDefault()
    const errMsg = valid(name, email, password, cf_password)
    if(errMsg) return dispatch({ type: 'NOTIFY', payload: {error: errMsg} })

    dispatch({ type: 'NOTIFY', payload: {loading: true} })

    const res = await postData('auth/register', userData)
    
    if(res.err) return dispatch({ type: 'NOTIFY', payload: {error: res.err} })

    return dispatch({ type: 'NOTIFY', payload: {success: res.msg} })
  }

  useEffect(() => {
    if(Object.keys(auth).length !== 0) router.push("/")
  }, [auth])

    return(
      <div>
        <Head>
          <title>Register Page</title>
        </Head>

        <form className="mx-auto my-4" style={{maxWidth: '500px'}} onSubmit={handleSubmit}>
          <div className="form-group">
            <label htmlFor="name">Name</label>
            <input type="text" className="form-control" id="name"
            name="name" value={name} onChange={handleChangeInput} />
          </div>

          <div className="form-group">
            <label htmlFor="exampleInputEmail1">Email address</label>
            <input type="email" className="form-control" id="exampleInputEmail1" aria-describedby="emailHelp"
            name="email" value={email} onChange={handleChangeInput} />
            <small id="emailHelp" className="form-text text-muted">We'll never share your email with anyone else.</small>
          </div>

          <div className="form-group">
            <label htmlFor="exampleInputPassword1">Password</label>
            <input type="password" className="form-control" id="exampleInputPassword1"
            name="password" value={password} onChange={handleChangeInput} />
          </div>

          <div className="form-group">
            <label htmlFor="exampleInputPassword2">Confirm Password</label>
            <input type="password" className="form-control" id="exampleInputPassword2"
            name="cf_password" value={cf_password} onChange={handleChangeInput} />
          </div>
          
          <button type="submit" className="btn btn-dark w-100">Register</button>

          <p className="my-2">
            Already have an account? <Link href="/signin"><a style={{color: 'crimson'}}>Login Now</a></Link>
          </p>
        </form>
      </div>
    )
  }
  
  export default Register


================================================
FILE: pages/signin.js
================================================
import Head from 'next/head'
import Link from 'next/link'
import {useState, useContext, useEffect} from 'react'
import {DataContext} from '../store/GlobalState'
import {postData} from '../utils/fetchData'
import Cookie from 'js-cookie'
import { useRouter } from 'next/router'

const Signin = () => {
  const initialState = { email: '', password: '' }
  const [userData, setUserData] = useState(initialState)
  const { email, password } = userData

  const {state, dispatch} = useContext(DataContext)
  const { auth } = state

  const router = useRouter()

  const handleChangeInput = e => {
    const {name, value} = e.target
    setUserData({...userData, [name]:value})
    dispatch({ type: 'NOTIFY', payload: {} })
  }

  const handleSubmit = async e => {
    e.preventDefault()
    dispatch({ type: 'NOTIFY', payload: {loading: true} })
    const res = await postData('auth/login', userData)
    
    if(res.err) return dispatch({ type: 'NOTIFY', payload: {error: res.err} })
    dispatch({ type: 'NOTIFY', payload: {success: res.msg} })

    dispatch({ type: 'AUTH', payload: {
      token: res.access_token,
      user: res.user
    }})

    Cookie.set('refreshtoken', res.refresh_token, {
      path: 'api/auth/accessToken',
      expires: 7
    })

    localStorage.setItem('firstLogin', true)
  }

  useEffect(() => {
    if(Object.keys(auth).length !== 0) router.push("/")
  }, [auth])

    return(
      <div>
        <Head>
          <title>Sign in Page</title>
        </Head>

        <form className="mx-auto my-4" style={{maxWidth: '500px'}} onSubmit={handleSubmit}>
          <div className="form-group">
            <label htmlFor="exampleInputEmail1">Email address</label>
            <input type="email" className="form-control" id="exampleInputEmail1" aria-describedby="emailHelp"
            name="email" value={email} onChange={handleChangeInput} />
            <small id="emailHelp" className="form-text text-muted">We'll never share your email with anyone else.</small>
          </div>
          <div className="form-group">
            <label htmlFor="exampleInputPassword1">Password</label>
            <input type="password" className="form-control" id="exampleInputPassword1"
            name="password" value={password} onChange={handleChangeInput} />
          </div>
          
          <button type="submit" className="btn btn-dark w-100">Login</button>

          <p className="my-2">
            You don't have an account? <Link href="/register"><a style={{color: 'crimson'}}>Register Now</a></Link>
          </p>
        </form>
      </div>
    )
  }
  
  export default Signin


================================================
FILE: pages/users.js
================================================
import Head from 'next/head'
import { useContext } from 'react'
import {DataContext} from '../store/GlobalState'
import Link from 'next/link'

const Users = () => {
    const {state, dispatch} = useContext(DataContext)
    const {users, auth, modal} = state

    if(!auth.user) return null;
    return(
        <div className="table-responsive">
            <Head>
                <title>Users</title>
            </Head>

            <table className="table w-100">
                <thead>
                    <tr>
                        <th></th>
                        <th>ID</th>
                        <th>Avatar</th>
                        <th>Name</th>
                        <th>Email</th>
                        <th>Admin</th>
                        <th>Action</th>
                    </tr>
                </thead>

                <tbody>
                    {
                        users.map((user, index)=> (
                            <tr key={user._id} style={{cursor: 'pointer'}}>
                                <th>{index + 1}</th>
                                <th>{user._id}</th>
                                <th>
                                    <img src={user.avatar} alt={user.avatar}
                                    style={{
                                        width: '30px', height: '30px', 
                                        overflow: 'hidden', objectFit: 'cover'
                                    }} />
                                </th>
                                <th>{user.name}</th>
                                <th>{user.email}</th>
                                <th>
                                    {
                                        user.role === 'admin'
                                        ? user.root ? <i className="fas fa-check text-success"> Root</i>
                                                    : <i className="fas fa-check text-success"></i>

                                        :<i className="fas fa-times text-danger"></i>
                                    }
                                </th>
                                <th>
                                    <Link href={
                                        auth.user.root && auth.user.email !== user.email
                                        ? `/edit_user/${user._id}` : '#!'
                                    }>
                                        <a><i className="fas fa-edit text-info mr-2" title="Edit"></i></a>
                                    </Link>

                                    {
                                        auth.user.root && auth.user.email !== user.email
                                        ? <i className="fas fa-trash-alt text-danger ml-2" title="Remove"
                                        data-toggle="modal" data-target="#exampleModal"
                                        onClick={() => dispatch({
                                            type: 'ADD_MODAL',
                                            payload: [{ data: users, id: user._id, title: user.name, type: 'ADD_USERS' }]
                                        })}></i>
                                        
                                        : <i className="fas fa-trash-alt text-danger ml-2" title="Remove"></i>
                                    }

                                </th>
                            </tr>
                        ))
                    }
                </tbody>
            </table>

        </div>
    )
}

export default Users


================================================
FILE: pages/api/auth/accessToken.js
================================================
import connectDB from '../../../utils/connectDB'
import Users from '../../../models/userModel'
import jwt from 'jsonwebtoken'
import { createAccessToken } from '../../../utils/generateToken'

connectDB()

export default async (req, res) => {
    try{
        const rf_token = req.cookies.refreshtoken;
        if(!rf_token) return res.status(400).json({err: 'Please login now!'})

        const result = jwt.verify(rf_token, process.env.REFRESH_TOKEN_SECRET)
        if(!result) return res.status(400).json({err: 'Your token is incorrect or has expired.'})

        const user = await Users.findById(result.id)
        if(!user) return res.status(400).json({err: 'User does not exist.'})

        const access_token = createAccessToken({id: user._id})
        res.json({
            access_token,
            user: {
                name: user.name,
                email: user.email,
                role: user.role,
                avatar: user.avatar,
                root: user.root
            }
        })
    }catch(err){
        return res.status(500).json({err: err.message})
    }
}




================================================
FILE: pages/api/auth/login.js
================================================
import connectDB from '../../../utils/connectDB'
import Users from '../../../models/userModel'
import bcrypt from 'bcrypt'
import { createAccessToken, createRefreshToken } from '../../../utils/generateToken'


connectDB()

export default async (req, res) => {
    switch(req.method){
        case "POST":
            await login(req, res)
            break;
    }
}

const login = async (req, res) => {
    try{
        const { email, password } = req.body

        const user = await Users.findOne({ email })
        if(!user) return res.status(400).json({err: 'This user does not exist.'})

        const isMatch = await bcrypt.compare(password, user.password)
        if(!isMatch) return res.status(400).json({err: 'Incorrect password.'})

        const access_token = createAccessToken({id: user._id})
        const refresh_token = createRefreshToken({id: user._id})
        
        res.json({
            msg: "Login Success!",
            refresh_token,
            access_token,
            user: {
                name: user.name,
                email: user.email,
                role: user.role,
                avatar: user.avatar,
                root: user.root
            }
        })

    }catch(err){
        return res.status(500).json({err: err.message})
    }
}


================================================
FILE: pages/api/auth/register.js
================================================
import connectDB from '../../../utils/connectDB'
import Users from '../../../models/userModel'
import valid from '../../../utils/valid'
import bcrypt from 'bcrypt'


connectDB()

export default async (req, res) => {
    switch(req.method){
        case "POST":
            await register(req, res)
            break;
    }
}

const register = async (req, res) => {
    try{
        const { name, email, password, cf_password } = req.body

        const errMsg = valid(name, email, password, cf_password)
        if(errMsg) return res.status(400).json({err: errMsg})

        const user = await Users.findOne({ email })
        if(user) return res.status(400).json({err: 'This email already exists.'})

        const passwordHash = await bcrypt.hash(password, 12)

        const newUser = new Users({ 
            name, email, password: passwordHash, cf_password 
        })

        await newUser.save()
        res.json({msg: "Register Success!"})

    }catch(err){
        return res.status(500).json({err: err.message})
    }
}


================================================
FILE: pages/api/categories/[id].js
================================================
import connectDB from '../../../utils/connectDB'
import Categories from '../../../models/categoriesModel'
import Products from '../../../models/productModel'
import auth from '../../../middleware/auth'

connectDB()

export default async (req, res) => {
    switch(req.method){
        case "PUT":
            await updateCategory(req, res)
            break;
        case "DELETE":
            await deleteCategory(req, res)
            break;
    }
}

const updateCategory = async (req, res) => {
    try {
        const result = await auth(req, res)
        if(result.role !== 'admin')
        return res.status(400).json({err: "Authentication is not valid."})

        const {id} = req.query
        const {name} = req.body

        const newCategory = await Categories.findOneAndUpdate({_id: id}, {name})
        res.json({
            msg: "Success! Update a new category",
            category: {
                ...newCategory._doc,
                name
            }
        })
    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}

const deleteCategory = async (req, res) => {
    try {
        const result = await auth(req, res)
        if(result.role !== 'admin')
        return res.status(400).json({err: "Authentication is not valid."})

        const {id} = req.query

        const products = await Products.findOne({category: id})
        if(products) return res.status(400).json({
            err: "Please delete all products with a relationship"
        })

        await Categories.findByIdAndDelete(id)
        
        res.json({msg: "Success! Deleted a category"})
    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}


================================================
FILE: pages/api/categories/index.js
================================================
import connectDB from '../../../utils/connectDB'
import Categories from '../../../models/categoriesModel'
import auth from '../../../middleware/auth'

connectDB()

export default async (req, res) => {
    switch(req.method){
        case "POST":
            await createCategory(req, res)
            break;
        case "GET":
            await getCategories(req, res)
            break;
    }
}

const createCategory = async (req, res) => {
    try {
        const result = await auth(req, res)
        if(result.role !== 'admin')
        return res.status(400).json({err: "Authentication is not valid."})

        const { name } = req.body
        if(!name) return res.status(400).json({err: "Name can not be left blank."})

        const newCategory = new Categories({name})

        await newCategory.save()
        res.json({
            msg: 'Success! Created a new category.',
            newCategory
        })

    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}

const getCategories = async (req, res) => {
    try {
        const categories = await Categories.find()

        res.json({categories})

    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}


================================================
FILE: pages/api/order/index.js
================================================
import connectDB from '../../../utils/connectDB'
import Orders from '../../../models/orderModel'
import Products from '../../../models/productModel'
import auth from '../../../middleware/auth'

connectDB()

export default async (req, res) => {
    switch(req.method){
        case "POST":
            await createOrder(req, res)
            break;
        case "GET":
            await getOrders(req, res)
            break;
    }
}

const getOrders = async (req, res) => {
    try {
        const result = await auth(req, res)

        let orders;
        if(result.role !== 'admin'){
            orders = await Orders.find({user: result.id}).populate("user", "-password")
        }else{
            orders = await Orders.find().populate("user", "-password")
        }

        res.json({orders})
    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}

const createOrder = async (req, res) => {
    try {
        const result = await auth(req, res)
        const { address, mobile, cart, total } = req.body

        const newOrder = new Orders({
            user: result.id, address, mobile, cart, total
        })

        cart.filter(item => {
            return sold(item._id, item.quantity, item.inStock, item.sold)
        })

        await newOrder.save()

        res.json({
            msg: 'Order success! We will contact you to confirm the order.',
            newOrder
        })

    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}

const sold = async (id, quantity, oldInStock, oldSold) => {
    await Products.findOneAndUpdate({_id: id}, {
        inStock: oldInStock - quantity,
        sold: quantity + oldSold
    })
}


================================================
FILE: pages/api/order/delivered/[id].js
================================================
import connectDB from '../../../../utils/connectDB'
import Orders from '../../../../models/orderModel'
import auth from '../../../../middleware/auth'

connectDB()

export default async (req, res) => {
    switch(req.method){
        case "PATCH":
            await deliveredOrder(req, res)
            break;
    }
}

const deliveredOrder = async(req, res) => {
    try {
        const result = await auth(req, res)
        if(result.role !== 'admin')
        return res.status(400).json({err: 'Authentication is not valid.'})
        const {id} = req.query


        const order = await Orders.findOne({_id: id})
        if(order.paid){
            await Orders.findOneAndUpdate({_id: id}, {delivered: true})
    
            res.json({
                msg: 'Updated success!',
                result: {
                    paid: true, 
                    dateOfPayment: order.dateOfPayment, 
                    method: order.method, 
                    delivered: true
                }
            })
        }else{
            await Orders.findOneAndUpdate({_id: id}, {
                paid: true, dateOfPayment: new Date().toISOString(), 
                method: 'Receive Cash', delivered: true
            })
    
            res.json({
                msg: 'Updated success!',
                result: {
                    paid: true, 
                    dateOfPayment: new Date().toISOString(), 
                    method: 'Receive Cash', 
                    delivered: true
                }
            })
        }
        
    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}


================================================
FILE: pages/api/order/payment/[id].js
================================================
import connectDB from '../../../../utils/connectDB'
import Orders from '../../../../models/orderModel'
import auth from '../../../../middleware/auth'

connectDB()

export default async (req, res) => {
    switch(req.method){
        case "PATCH":
            await paymentOrder(req, res)
            break;
    }
}

const paymentOrder = async(req, res) => {
    try {
        const result = await auth(req, res)
        
        if(result.role === 'user'){
            const {id} = req.query
            const { paymentId } = req.body
    
            await Orders.findOneAndUpdate({_id: id}, {
                paid: true, dateOfPayment: new Date().toISOString(), paymentId,
                method: 'Paypal'
            })
    
            res.json({msg: 'Payment success!'})
        }
        
    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}


================================================
FILE: pages/api/product/[id].js
================================================
import connectDB from '../../../utils/connectDB'
import Products from '../../../models/productModel'
import auth from '../../../middleware/auth'

connectDB()

export default async (req, res) => {
    switch(req.method){
        case "GET":
            await getProduct(req, res)
            break;
        case "PUT":
            await updateProduct(req, res)
            break;
        case "DELETE":
            await deleteProduct(req, res)
            break;
    }
}

const getProduct = async (req, res) => {
    try {
        const { id } = req.query;

        const product = await Products.findById(id)
        if(!product) return res.status(400).json({err: 'This product does not exist.'})
        
        res.json({ product })

    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}

const updateProduct = async (req, res) => {
    try {
        const result = await auth(req, res)
        if(result.role !== 'admin') 
        return res.status(400).json({err: 'Authentication is not valid.'})

        const {id} = req.query
        const {title, price, inStock, description, content, category, images} = req.body

        if(!title || !price || !inStock || !description || !content || category === 'all' || images.length === 0)
        return res.status(400).json({err: 'Please add all the fields.'})

        await Products.findOneAndUpdate({_id: id}, {
            title: title.toLowerCase(), price, inStock, description, content, category, images
        })

        res.json({msg: 'Success! Updated a product'})
    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}

const deleteProduct = async(req, res) => {
    try {
        const result = await auth(req, res)
        
        if(result.role !== 'admin') 
        return res.status(400).json({err: 'Authentication is not valid.'})

        const {id} = req.query

        await Products.findByIdAndDelete(id)
        res.json({msg: 'Deleted a product.'})

    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}


================================================
FILE: pages/api/product/index.js
================================================
import connectDB from '../../../utils/connectDB'
import Products from '../../../models/productModel'
import auth from '../../../middleware/auth'

connectDB()

export default async (req, res) => {
    switch(req.method){
        case "GET":
            await getProducts(req, res)
            break;
        case "POST":
            await createProduct(req, res)
            break;
    }
}

class APIfeatures {
    constructor(query, queryString){
        this.query = query;
        this.queryString = queryString;
    }
    filtering(){
        const queryObj = {...this.queryString}

        const excludeFields = ['page', 'sort', 'limit']
        excludeFields.forEach(el => delete(queryObj[el]))

        if(queryObj.category !== 'all')
            this.query.find({category: queryObj.category})
        if(queryObj.title !== 'all')
            this.query.find({title: {$regex: queryObj.title}})

        this.query.find()
        return this;
    }

    sorting(){
        if(this.queryString.sort){
            const sortBy = this.queryString.sort.split(',').join('')
            this.query = this.query.sort(sortBy)
        }else{
            this.query = this.query.sort('-createdAt')
        }

        return this;
    }

    paginating(){
        const page = this.queryString.page * 1 || 1
        const limit = this.queryString.limit * 1 || 6
        const skip = (page - 1) * limit;
        this.query = this.query.skip(skip).limit(limit)
        return this;
    }
}

const getProducts = async (req, res) => {
    try {
        const features = new APIfeatures(Products.find(), req.query)
        .filtering().sorting().paginating()

        const products = await features.query
        
        res.json({
            status: 'success',
            result: products.length,
            products
        })
    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}

const createProduct = async (req, res) => {
    try {
        const result = await auth(req, res)
        if(result.role !== 'admin') return res.status(400).json({err: 'Authentication is not valid.'})

        const {title, price, inStock, description, content, category, images} = req.body

        if(!title || !price || !inStock || !description || !content || category === 'all' || images.length === 0)
        return res.status(400).json({err: 'Please add all the fields.'})


        const newProduct = new Products({
            title: title.toLowerCase(), price, inStock, description, content, category, images
        })

        await newProduct.save()

        res.json({msg: 'Success! Created a new product'})

    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}


================================================
FILE: pages/api/user/[id].js
================================================
import connectDB from '../../../utils/connectDB'
import Users from '../../../models/userModel'
import auth from '../../../middleware/auth'

connectDB()

export default async (req, res) => {
    switch(req.method){
        case "PATCH":
            await updateRole(req, res)
            break;
        case "DELETE":
            await deleteUser(req, res)
            break;
    }
}

const updateRole = async (req, res) => {
    try {
       const result = await auth(req, res)
       if(result.role !== 'admin' || !result.root) 
       return res.status(400).json({err: "Authentication is not valid"})

       const {id} = req.query
       const {role} = req.body

       await Users.findOneAndUpdate({_id: id}, {role})
       res.json({msg: 'Update Success!'})

    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}

const deleteUser = async (req, res) => {
    try {
       const result = await auth(req, res)
       if(result.role !== 'admin' || !result.root) 
       return res.status(400).json({err: "Authentication is not valid"})

       const {id} = req.query

       await Users.findByIdAndDelete(id)
       res.json({msg: 'Deleted Success!'})

    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}


================================================
FILE: pages/api/user/index.js
================================================
import connectDB from '../../../utils/connectDB'
import Users from '../../../models/userModel'
import auth from '../../../middleware/auth'

connectDB()

export default async (req, res) => {
    switch(req.method){
        case "PATCH":
            await uploadInfor(req, res)
            break;
        case "GET":
            await getUsers(req, res)
            break;
    }
}

const getUsers = async (req, res) => {
    try {
       const result = await auth(req, res)
       if(result.role !== 'admin') 
       return res.status(400).json({err: "Authentication is not valid"})

        const users = await Users.find().select('-password')
        res.json({users})

    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}


const uploadInfor = async (req, res) => {
    try {
        const result = await auth(req, res)
        const {name, avatar} = req.body

        const newUser = await Users.findOneAndUpdate({_id: result.id}, {name, avatar})

        res.json({
            msg: "Update Success!",
            user: {
                name,
                avatar,
                email: newUser.email,
                role: newUser.role
            }
        })
    } catch (err) {
        return res.status(500).json({err: err.message})
    }
}


================================================
FILE: pages/api/user/resetPassword.js
================================================
import connectDB from '../../../utils/connectDB'
import Users from '../../../models/userModel'
import auth from '../../../middleware/auth'
import bcrypt from 'bcrypt'

connectDB()

export default async (req, res) => {
    switch(req.method){
        case "PATCH":
            await resetPassword(req, res)
            break;
    }
}


const resetPassword = async (req, res) => {
    try {
        const result = await auth(req, res)
        const { password } = req.body
        const passwordHash = await bcrypt.hash(password, 12)

        await Users.findOneAndUpdate({_id: result.id}, {password: passwordHash})

        res.json({ msg: "Update Success!"})
        
    } catch (err) {
        return res.status(500).json({err: err.message})
    }   
}


================================================
FILE: pages/create/[[...id]].js
================================================
import Head from 'next/head'
import {useState, useContext, useEffect} from 'react'
import {DataContext} from '../../store/GlobalState'
import {imageUpload} from '../../utils/imageUpload'
import {postData, getData, putData} from '../../utils/fetchData'
import {useRouter} from 'next/router'

const ProductsManager = () => {
    const initialState = {
        title: '',
        price: 0,
        inStock: 0,
        description: '',
        content: '',
        category: ''
    }
    const [product, setProduct] = useState(initialState)
    const {title, price, inStock, description, content, category} = product

    const [images, setImages] = useState([])

    const {state, dispatch} = useContext(DataContext)
    const {categories, auth} = state

    const router = useRouter()
    const {id} = router.query
    const [onEdit, setOnEdit] = useState(false)

    useEffect(() => {
        if(id){
            setOnEdit(true)
            getData(`product/${id}`).then(res => {
                setProduct(res.product)
                setImages(res.product.images)
            })
        }else{
            setOnEdit(false)
            setProduct(initialState)
            setImages([])
        }
    },[id])

    const handleChangeInput = e => {
        const {name, value} = e.target
        setProduct({...product, [name]:value})
        dispatch({type: 'NOTIFY', payload: {}})
    }

    const handleUploadInput = e => {
        dispatch({type: 'NOTIFY', payload: {}})
        let newImages = []
        let num = 0
        let err = ''
        const files = [...e.target.files]

        if(files.length === 0) 
        return dispatch({type: 'NOTIFY', payload: {error: 'Files does not exist.'}})

        files.forEach(file => {
            if(file.size > 1024 * 1024)
            return err = 'The largest image size is 1mb'

            if(file.type !== 'image/jpeg' && file.type !== 'image/png')
            return err = 'Image format is incorrect.'

            num += 1;
            if(num <= 5) newImages.push(file)
            return newImages;
        })

        if(err) dispatch({type: 'NOTIFY', payload: {error: err}})

        const imgCount = images.length
        if(imgCount + newImages.length > 5)
        return dispatch({type: 'NOTIFY', payload: {error: 'Select up to 5 images.'}})
        setImages([...images, ...newImages])
    }

    const deleteImage = index => {
        const newArr = [...images]
        newArr.splice(index, 1)
        setImages(newArr)
    }

    const handleSubmit = async(e) => {
        e.preventDefault()
        if(auth.user.role !== 'admin') 
        return dispatch({type: 'NOTIFY', payload: {error: 'Authentication is not valid.'}})

        if(!title || !price || !inStock || !description || !content || category === 'all' || images.length === 0)
        return dispatch({type: 'NOTIFY', payload: {error: 'Please add all the fields.'}})

    
        dispatch({type: 'NOTIFY', payload: {loading: true}})
        let media = []
        const imgNewURL = images.filter(img => !img.url)
        const imgOldURL = images.filter(img => img.url)

        if(imgNewURL.length > 0) media = await imageUpload(imgNewURL)

        let res;
        if(onEdit){
            res = await putData(`product/${id}`, {...product, images: [...imgOldURL, ...media]}, auth.token)
            if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})
        }else{
            res = await postData('product', {...product, images: [...imgOldURL, ...media]}, auth.token)
            if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})
        }

        return dispatch({type: 'NOTIFY', payload: {success: res.msg}})
        
    }

    return(
        <div className="products_manager">
            <Head>
                <title>Products Manager</title>
            </Head>
            <form className="row" onSubmit={handleSubmit}>
                <div className="col-md-6">
                    
                    <input type="text" name="title" value={title}
                    placeholder="Title" className="d-block my-4 w-100 p-2"
                    onChange={handleChangeInput} />

                    <div className="row">
                        <div className="col-sm-6">
                            <label htmlFor="price">Price</label>
                            <input type="number" name="price" value={price}
                            placeholder="Price" className="d-block w-100 p-2"
                            onChange={handleChangeInput} />
                        </div>

                        <div className="col-sm-6">
                            <label htmlFor="price">In Stock</label>
                            <input type="number" name="inStock" value={inStock}
                            placeholder="inStock" className="d-block w-100 p-2"
                            onChange={handleChangeInput} />
                        </div>
                    </div>

                    <textarea name="description" id="description" cols="30" rows="4"
                    placeholder="Description" onChange={handleChangeInput}
                    className="d-block my-4 w-100 p-2" value={description} />

                    <textarea name="content" id="content" cols="30" rows="6"
                    placeholder="Content" onChange={handleChangeInput}
                    className="d-block my-4 w-100 p-2" value={content} />

                    <div className="input-group-prepend px-0 my-2">
                        <select name="category" id="category" value={category}
                        onChange={handleChangeInput} className="custom-select text-capitalize">
                            <option value="all">All Products</option>
                            {
                                categories.map(item => (
                                    <option key={item._id} value={item._id}>
                                        {item.name}
                                    </option>
                                ))
                            }
                        </select>
                    </div>

                    <button type="submit" className="btn btn-info my-2 px-4">
                        {onEdit ? 'Update': 'Create'}
                    </button>

                </div>

                <div className="col-md-6 my-4">
                    <div className="input-group mb-3">
                        <div className="input-group-prepend">
                            <span className="input-group-text">Upload</span>
                        </div>
                        <div className="custom-file border rounded">
                            <input type="file" className="custom-file-input"
                            onChange={handleUploadInput} multiple accept="image/*" />
                        </div>

                    </div> 

                    <div className="row img-up mx-0">
                        {
                            images.map((img, index) => (
                                <div key={index} className="file_img my-1">
                                    <img src={img.url ? img.url : URL.createObjectURL(img)}
                                     alt="" className="img-thumbnail rounded" />

                                     <span onClick={() => deleteImage(index)}>X</span>
                                </div>
                            ))
                        }
                    </div>
                        

                </div>

               
            </form>

            
        </div>
    )
}

export default ProductsManager


================================================
FILE: pages/edit_user/[id].js
================================================
import Head from 'next/head'
import { useContext, useState, useEffect } from 'react'
import {DataContext} from '../../store/GlobalState'
import {updateItem} from '../../store/Actions'

import {useRouter} from 'next/router'
import {patchData} from '../../utils/fetchData'

const EditUser = () => {
    const router = useRouter()
    const { id }= router.query

    const {state, dispatch} = useContext(DataContext)
    const {auth, users} = state

    const [editUser, setEditUser] = useState([])
    const [checkAdmin, setCheckAdmin] = useState(false)
    const [num, setNum] = useState(0)

    useEffect(() => {
        users.forEach(user => {
            if(user._id === id){
                setEditUser(user)
                setCheckAdmin(user.role === 'admin' ? true : false)
            }
        })
    },[users])

    const handleCheck = () => {
        setCheckAdmin(!checkAdmin)
        setNum(num + 1)
    }

    const handleSubmit = () => {
        let role = checkAdmin ? 'admin' : 'user'
        if(num % 2 !== 0){
            dispatch({type: 'NOTIFY', payload: {loading: true}})
            patchData(`user/${editUser._id}`, {role}, auth.token)
            .then(res => {
                if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})

                dispatch(updateItem(users, editUser._id, {
                    ...editUser, role
                }, 'ADD_USERS'))

                return dispatch({type: 'NOTIFY', payload: {success: res.msg}})
            })
        }
        
    }

    
    return (
        <div className="edit_user my-3">
            <Head>
                <title>Edit User</title>
            </Head>

            <div>
                <button className="btn btn-dark" onClick={() => router.back()}>
                    <i className="fas fa-long-arrow-alt-left" aria-hidden></i> Go Back
                </button>
            </div>

            <div className="col-md-4 mx-auto my-4">
                <h2 className="text-uppercase text-secondary">Edit User</h2>

                <div className="form-group">
                    <label htmlFor="name" className="d-block">Name</label>
                    <input type="text" id="name" defaultValue={editUser.name} disabled />
                </div>

                <div className="form-group">
                    <label htmlFor="email" className="d-block">Email</label>
                    <input type="text" id="email" defaultValue={editUser.email} disabled />
                </div>

                <div className="form-group">
                    <input type="checkbox" id="isAdmin" checked={checkAdmin}
                    style={{width: '20px', height: '20px'}} onChange={handleCheck} />

                    <label htmlFor="isAdmin" style={{transform: 'translate(4px, -3px)'}}>
                        isAdmin
                    </label>
                </div>

                <button className="btn btn-dark" onClick={handleSubmit}>Update</button>

            </div>

        </div>
    )
}

export default EditUser


================================================
FILE: pages/order/[id].js
================================================
import Head from 'next/head'
import { useState, useContext, useEffect } from 'react'
import { DataContext } from '../../store/GlobalState'
import { useRouter } from 'next/router'
import Link from 'next/link'
import OrderDetail from '../../components/OrderDetail'


const DetailOrder = () => {
    const {state, dispatch} = useContext(DataContext)
    const {orders, auth} = state

    const router = useRouter()

    const [orderDetail, setOrderDetail] = useState([])

    useEffect(() => {
        const newArr = orders.filter(order => order._id === router.query.id)
        setOrderDetail(newArr)
    },[orders])
            
    if(!auth.user) return null;
    return(
        <div className="my-3">
            <Head>
                <title>Detail Orders</title>
            </Head>

            <div>
                <button className="btn btn-dark" onClick={() => router.back()}>
                    <i className="fas fa-long-arrow-alt-left"  aria-hidden="true"></i> Go Back
                </button>
            </div>
            
            <OrderDetail orderDetail={orderDetail} state={state} dispatch={dispatch} />
        
        </div>
    )
}

export default DetailOrder


================================================
FILE: pages/product/[id].js
================================================
import Head from 'next/head'
import { useState, useContext } from 'react'
import { getData } from '../../utils/fetchData'
import { DataContext } from '../../store/GlobalState'
import { addToCart } from '../../store/Actions'

const DetailProduct = (props) => {
    const [product] = useState(props.product)
    const [tab, setTab] = useState(0)

    const { state, dispatch } = useContext(DataContext)
    const { cart } = state

    const isActive = (index) => {
        if(tab === index) return " active";
        return ""
    }

    return(
        <div className="row detail_page">
            <Head>
                <title>Detail Product</title>
            </Head>

            <div className="col-md-6">
                <img src={ product.images[tab].url } alt={ product.images[tab].url }
                className="d-block img-thumbnail rounded mt-4 w-100"
                style={{height: '350px'}} />

                <div className="row mx-0" style={{cursor: 'pointer'}} >

                    {product.images.map((img, index) => (
                        <img key={index} src={img.url} alt={img.url}
                        className={`img-thumbnail rounded ${isActive(index)}`}
                        style={{height: '80px', width: '20%'}}
                        onClick={() => setTab(index)} />
                    ))}

                </div>
            </div>

            <div className="col-md-6 mt-3">
                <h2 className="text-uppercase">{product.title}</h2>
                <h5 className="text-danger">${product.price}</h5>

                <div className="row mx-0 d-flex justify-content-between">
                    {
                        product.inStock > 0
                        ? <h6 className="text-danger">In Stock: {product.inStock}</h6>
                        : <h6 className="text-danger">Out Stock</h6>
                    }

                    <h6 className="text-danger">Sold: {product.sold}</h6>
                </div>

                <div className="my-2">{product.description}</div>
                <div className="my-2">
                    {product.content}
                </div>

                <button type="button" className="btn btn-dark d-block my-3 px-5"
                onClick={() => dispatch(addToCart(product, cart))} >
                    Buy
                </button>

            </div>
        </div>
    )
}

export async function getServerSideProps({params: {id}}) {

    const res = await getData(`product/${id}`)
    // server side rendering
    return {
      props: { product: res.product }, // will be passed to the page component as props
    }
}


export default DetailProduct



================================================
FILE: store/Actions.js
================================================
export const ACTIONS = {
    NOTIFY: 'NOTIFY',
    AUTH: 'AUTH',
    ADD_CART: 'ADD_CART',
    ADD_MODAL: 'ADD_MODAL',
    ADD_ORDERS: 'ADD_ORDERS',
    ADD_USERS: 'ADD_USERS',
    ADD_CATEGORIES: 'ADD_CATEGORIES',
}

export const addToCart = (product, cart) => {
    if(product.inStock === 0)
    return ({ type: 'NOTIFY', payload: {error: 'This product is out of stock.'} }) 

    const check = cart.every(item => {
        return item._id !== product._id
    })

    if(!check) return ({ type: 'NOTIFY', payload: {error: 'The product has been added to cart.'} }) 

    return ({ type: 'ADD_CART', payload: [...cart, {...product, quantity: 1}] }) 
}

export const decrease = (data, id) => {
    const newData = [...data]
    newData.forEach(item => {
        if(item._id === id) item.quantity -= 1
    })

    return ({ type: 'ADD_CART', payload: newData })
}

export const increase = (data, id) => {
    const newData = [...data]
    newData.forEach(item => {
        if(item._id === id) item.quantity += 1
    })

    return ({ type: 'ADD_CART', payload: newData })
}


export const deleteItem = (data, id, type) => {
    const newData = data.filter(item => item._id !== id)
    return ({ type, payload: newData})
}

export const updateItem = (data, id, post, type) => {
    const newData = data.map(item => (item._id === id ? post : item))
    return ({ type, payload: newData})
}


================================================
FILE: store/GlobalState.js
================================================
import { createContext, useReducer, useEffect } from 'react'
import reducers from './Reducers'
import { getData } from '../utils/fetchData'


export const DataContext = createContext()


export const DataProvider = ({children}) => {
    const initialState = { 
        notify: {}, auth: {}, cart: [], modal: [], orders: [], users: [], categories: []
    }

    const [state, dispatch] = useReducer(reducers, initialState)
    const { cart, auth } = state

    useEffect(() => {
        const firstLogin = localStorage.getItem("firstLogin");
        if(firstLogin){
            getData('auth/accessToken').then(res => {
                if(res.err) return localStorage.removeItem("firstLogin")
                dispatch({ 
                    type: "AUTH",
                    payload: {
                        token: res.access_token,
                        user: res.user
                    }
                })
            })
        }

        getData('categories').then(res => {
            if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})

            dispatch({ 
                type: "ADD_CATEGORIES",
                payload: res.categories
            })
        })
        
    },[])

    useEffect(() => {
        const __next__cart01__devat = JSON.parse(localStorage.getItem('__next__cart01__devat'))

        if(__next__cart01__devat) dispatch({ type: 'ADD_CART', payload: __next__cart01__devat })
    }, [])

    useEffect(() => {
        localStorage.setItem('__next__cart01__devat', JSON.stringify(cart))
    }, [cart])

    useEffect(() => {
        if(auth.token){
            getData('order', auth.token)
            .then(res => {
                if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})
                
                dispatch({type: 'ADD_ORDERS', payload: res.orders})
            })

            if(auth.user.role === 'admin'){
                getData('user', auth.token)
                .then(res => {
                    if(res.err) return dispatch({type: 'NOTIFY', payload: {error: res.err}})
                
                    dispatch({type: 'ADD_USERS', payload: res.users})
                })
            }
        }else{
            dispatch({type: 'ADD_ORDERS', payload: []})
            dispatch({type: 'ADD_USERS', payload: []})
        }
    },[auth.token])

    return(
        <DataContext.Provider value={{state, dispatch}}>
            {children}
        </DataContext.Provider>
    )
}


================================================
FILE: store/Reducers.js
================================================
import { ACTIONS } from './Actions'


const reducers = (state, action) => {
    switch(action.type){
        case ACTIONS.NOTIFY:
            return {
                ...state,
                notify: action.payload
            };
        case ACTIONS.AUTH:
            return {
                ...state,
                auth: action.payload
            };
        case ACTIONS.ADD_CART:
            return {
                ...state,
                cart: action.payload
            };
        case ACTIONS.ADD_MODAL:
            return {
                ...state,
                modal: action.payload
            };
        case ACTIONS.ADD_ORDERS:
            return {
                ...state,
                orders: action.payload
            };
        case ACTIONS.ADD_USERS:
            return {
                ...state,
                users: action.payload
            };
        case ACTIONS.ADD_CATEGORIES:
            return {
                ...state,
                categories: action.payload
            };
        default:
            return state;
    }
}

export default reducers


================================================
FILE: styles/globals.css
================================================
html,
body {
  padding: 0;
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, Segoe UI, Roboto, Oxygen,
    Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, sans-serif;
}

a {
  color: inherit;
  text-decoration: none;
}

* {
  box-sizing: border-box;
}

img{
  object-fit: cover;
  object-position: 0 30%;
}

/* ----------- Loading ------------- */
@import url("./loading.css");

/* ----------- Products ------------- */
@import url("./product.css");

/* ----------- Profile ------------- */
@import url("./profile.css");

/* ----------- Products Manager ------------- */
@import url("./products_manager.css");


================================================
FILE: styles/loading.css
================================================
.loading{
    display: flex;
    justify-content: center;
    align-items: center;
}
.loading svg{
    font-size: 5px;
    font-weight: 900;
    text-transform: uppercase;
    letter-spacing: 1px;
    animation: text 1s ease-in-out infinite;
}
@keyframes text{
    50%{ opacity: 0.1;}
}
.loading polygon{
    stroke-dasharray: 22;
    stroke-dashoffset: 1;
    animation: dash 4s cubic-bezier(0.455, 0.03, 0.515, 0.955)
    infinite alternate-reverse;
}
@keyframes dash{
    to{stroke-dashoffset: 234;}
}


================================================
FILE: styles/product.css
================================================
.products{
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(290px, 1fr));
    justify-content: center;
    margin: 20px 0;
}
.products .card{
    margin: 10px auto;
}
.products .card-img-top{
    height: 250px;
    width: 100%;
}
.products .card-title{
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
}
.products .card-text{
    height: 70px;
    overflow: hidden;
    -webkit-box-orient: vertical;
    -webkit-line-clamp: 3;
}


/* --------- Detail Product ----------- */
.detail_page .img-thumbnail.active{
    border-color: orangered;
    padding: 2px;
}


================================================
FILE: styles/products_manager.css
================================================
.img-up .file_img{
    position: relative;
    width: 25%;
    height: 100px;
    cursor: pointer;
}
.img-up .file_img:nth-of-type(1){
    width: 100%;
    height: 400px;
}
.img-up .file_img img{
    width: 100%;
    height: 100%;
    display: block;
    object-fit: cover;
}
.img-up .file_img span{
    position: absolute;
    top: -1px;
    right: -2px;
    z-index: 4;
    background: white;
    color: crimson;
    padding: 3px 7px;
    border-radius: 50%;
    font-size: 10px;
    font-weight: bolder;
    border: 1px solid crimson;
}


================================================
FILE: styles/profile.css
================================================
.profile_page .avatar{
    width: 150px;
    height: 150px;
    overflow: hidden;
    position: relative;
    margin: 15px auto;
    border: 1px solid #ddd;
    border-radius: 50%;
    cursor: pointer;
}

.profile_page .avatar img{
    width: 100%;
    height: 100%;
    display: block;
    object-fit: cover;
}

.profile_page .avatar span{
    position: absolute;
    bottom: -100%;
    left: 0;
    width: 100%;
    height: 50%;
    background: #fff8;
    text-align: center;
    font-weight: 400;
    color: rgb(255, 140, 45);
    transition: 0.3 ease-in-out;
}

.profile_page .avatar:hover span{
    bottom: -15%;
}

.profile_page #file_up{
    position: absolute;
    top:0;
    left: 0;
    width: 100%;
    height: 100%;
    opacity: 0;
    cursor: pointer;
}
::-webkit-file-upload-button{
    cursor: pointer;
}




================================================
FILE: utils/connectDB.js
================================================
import mongoose from 'mongoose'

const connectDB = () => {
    if(mongoose.connections[0].readyState){
        console.log('Already connected.')
        return;
    }
    mongoose.connect(process.env.MONGODB_URL, {
        useCreateIndex: true,
        useFindAndModify: false,
        useNewUrlParser: true,
        useUnifiedTopology: true
    }, err => {
        if(err) throw err;
        console.log('Connected to mongodb.')
    })
}


export default connectDB


================================================
FILE: utils/fetchData.js
================================================
const baseUrl = process.env.BASE_URL

export const getData = async (url, token) => {
    const res = await fetch(`${baseUrl}/api/${url}`, {
        method: 'GET',
        headers: {
            'Authorization': token
        }
    })

    const data = await res.json()
    return data
}

export const postData = async (url, post, token) => {
    const res = await fetch(`${baseUrl}/api/${url}`, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': token
        },
        body: JSON.stringify(post)
    })

    const data = await res.json()
    return data
}



export const putData = async (url, post, token) => {
    const res = await fetch(`${baseUrl}/api/${url}`, {
        method: 'PUT',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': token
        },
        body: JSON.stringify(post)
    })

    const data = await res.json()
    return data
}

export const patchData = async (url, post, token) => {
    const res = await fetch(`${baseUrl}/api/${url}`, {
        method: 'PATCH',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': token
        },
        body: JSON.stringify(post)
    })

    const data = await res.json()
    return data
}


export const deleteData = async (url, token) => {
    const res = await fetch(`${baseUrl}/api/${url}`, {
        method: 'DELETE',
        headers: {
            'Content-Type': 'application/json',
            'Authorization': token
        }
    })

    const data = await res.json()
    return data
}


================================================
FILE: utils/filterSearch.js
================================================
const filterSearch = ({router, page, category, sort, search}) => {
    const path = router.pathname;
    const query = router.query;


    if(category) query.category = category;
    if(page) query.page = page;
    if(search) query.search = search;
    if(sort) query.sort = sort;

    router.push({
        pathname: path,
        query: query
    })
}

export default filterSearch


================================================
FILE: utils/generateToken.js
================================================
import jwt from 'jsonwebtoken'

export const createAccessToken = (payload) => {
    console.log(process.env.ACCESS_TOKEN_SECRET)
    return jwt.sign(payload, process.env.ACCESS_TOKEN_SECRET, {expiresIn: '15m'})
}

export const createRefreshToken = (payload) => {
    return jwt.sign(payload, process.env.REFRESH_TOKEN_SECRET, {expiresIn: '7d'})
}


================================================
FILE: utils/imageUpload.js
================================================
export const imageUpload = async (images) => {
    let imgArr = []
    for(const item of images){
        const formData = new FormData()
        formData.append("file", item)
        formData.append("upload_preset", process.env.CLOUD_UPDATE_PRESET)
        formData.append("cloud_name", process.env.CLOUD_NAME)

        const res = await fetch(process.env.CLOUD_API, {
            method: "POST",
            body: formData
        })

        const data = await res.json()
        imgArr.push({public_id: data.public_id, url: data.secure_url})
    }
    return imgArr;
}


================================================
FILE: utils/valid.js
================================================
const valid = (name, email, password, cf_password) => {
    if(!name || !email || !password)
    return 'Please add all fields.'

    if(!validateEmail(email))
    return 'Invalid emails.'

    if(password.length < 6)
    return 'Password must be at least 6 characters.'

    if(password !== cf_password)
    return 'Confirm password did not match.'
}


function validateEmail(email) {
    const re = /^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
    return re.test(email);
}

export default valid

