# Learn React Http Request

## Rangkuman Belajar

## Permulaan
- Untuk mendapatkan sample dari API dengan respon `.json`, bisa diakses di https://jsonplaceholder.typicode.com/
- Untuk http requestnya menggunakan package `axios`. 
see documentation axios at https://github.com/axios/axios



### Kapan waktu terbaik melakukan request data?

#### Inisiasi Component
Lihat pada component lifecycle,
<center>
  <img src='./docs/images/react-component-lifecycle.png' style='max-width: 800px'>
</center>

dari `lifecycle` tersebut, waktu terbaik untuk melakukan `SIDE EFFECT` atau `request data` dari endpoint tertentu adalah pada saat `componentDidMount()`. Kok bisa? baca dokumentasi reactnya,
> componentDidMount() is invoked immediately after a component is mounted (inserted into the tree). Initialization that requires DOM nodes should go here. If you need to load data from a remote endpoint, this is a good place to instantiate the network request.

See more: https://reactjs.org/docs/react-component.html#componentdidmount

#### Updated Component
<center>
  <img src='./docs/images/update-lifecycle.png' style='max-width: 800px'>
</center>

sama halnya melakukan request di inisiasi component. Saat melakukan update component (kasus `fetch` detail data by id post) perlu dipertimbangkan `component lifecycle update`-nya. Jika dilihat pada gambar diatas, maka sesi terbaik adalah saat `componentDidUpdate()` dan tentu harus dipertimbangkan berbagai kondisi agar saat melakukan set state tidak berulang tanpa ada hentinya.

```js
 // sample di component FullPost.js
    componentDidUpdate () {
        if(this.props.id) {
            if (this.state.loadedPost && this.state.loadedPost.id === this.props.id) return;
            axios.get('https://jsonplaceholder.typicode.com/posts/' + this.props.id)
                .then(res => {
                    this.setState({ loadedPost: res.data })
                })
                .catch(err => {
                    console.log(err);
                })
        }
    }
```