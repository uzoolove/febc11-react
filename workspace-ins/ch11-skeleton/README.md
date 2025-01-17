# Lion Board 앱

## 샘플 페이지 확인
* sample/07/workspace/ch11-skeleton 폴더를 workspace에 복사
* workspace/ch11-skeleton/sample 폴더에서 다음 명령 실행
```sh
npx serve .
```

* 브라우저에서 페이지 이동 테스트

# 1단계
* HTML 코드를 기반으로 리액트 컴포넌트 생성
* 리액트 라우터 적용

## 프로젝트 생성
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch02-start#vite
* workspace/ch11-skeleton 폴더에서 다음 명령 실행
  ```sh
  npm init vite@latest
  ```
  - Project name: lion-board
  - Select a framework: React
  - Select a variant: JavaScript

  ```sh
  # 생성한 프로젝트 폴더로 이동
  cd lion-board
  # 기본 패키지 설치
  npm i
  # 추가 패키지 설치
  npm i prop-types react-router-dom react-hook-form axios @tanstack/react-query @tanstack/react-query-devtools react-spinners react-toastify zustand react-helmet-async
  npm i -D tailwindcss postcss autoprefixer
  # 개발 서버 실행
  npm run dev
  ```

## 프로젝트 설정
### alias 추가
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch02-start#viteconfigjs

#### vite.config.js
```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: [
      { find: "@", replacement: "/src" },
      { find: "@components", replacement: "/src/components" },
      { find: "@pages", replacement: "/src/pages" },
      { find: "@hooks", replacement: "/src/hooks" },
      { find: "@zustand", replacement: "/src/zustand" },
    ],
  },
})
```
#### jsconfig.json
```json
{
  "compilerOptions": {
    "baseUrl": "./src",
    "paths": {
      "@/*": ["/*"],
      "@components/*": ["components/*"],
      "@pages/*": ["pages/*"],
      "@hooks/*": ["hooks/*"],
      "@zustand/*": ["zustand/*"],
    }
  }
}
```

### Tailwind CSS 설정
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch08-css#%EC%84%A4%EC%A0%95-%ED%8C%8C%EC%9D%BC-%EC%83%9D%EC%84%B1

* tailwind.config.js
```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ["index.html", "./src/**/*.{js,jsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

* src/index.css
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

## 정적인 자원 처리
* sample/images 폴더를 lion-board/public 폴더에 복사
* favicon 설정
  - lion-board/index.html 파일의 favicon 설정 수정
  ```html
  <link rel="icon" type="image/svg+xml" href="/images/favicon.svg" />
  ```

## UI 컴포넌트 작성
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch02-start#2-4-jsx
* workspace/ch11-skeleton/sample 폴더의 html 코드를 컴포넌트로 이동
  - header 태그는 Header.jsx에서 사용
  - footer 태그는 Footer.jsx에서 사용
  - div id="main" 태그는 각 페이지의 컴포넌트에서 사용
  - JSX 문법에 맞게 수정

### 공통 컴포넌트
* lion-board/src/components/layout 폴더 생성후 파일 작성
* Header.jsx
  - sample/index.html의 `<header>` 영역 복사
  - JSX 문법에 맞게 수정

* Footer.jsx
  - sample/index.html의 `<footer>` 영역 복사
  - JSX 문법에 맞게 수정

### 레이아웃 컴포넌트 작성
* lion-board/src/components/layout/index.jsx 파일 작성
  ```jsx
  import Footer from "@components/Footer";
  import Header from "@components/Header";
  import { Outlet } from 'react-router-dom';

  export default function Layout(){
    return (
      <div className="flex flex-col min-h-screen dark:bg-gray-700 dark:text-gray-200 transition-color duration-500 ease-in-out">
        <Header />
        <Outlet />
        <Footer />
      </div>
    );
  }
  ```

### 페이지별 컴포넌트
#### 메인 페이지
* sample/index.html 참고해서 lion-board/src/pages/index.jsx 파일 작성

#### 게시판 기능
* lion-board/src/pages/board 폴더 생성후 sample 폴더 html 파일의 `<main>` 태그 복사해서 완성
  - List.jsx
  - ListItem.jsx
  - New.jsx
  - Detail.jsx
  - Edit.jsx
  - CommentList.jsx
  - CommentListItem.jsx
  - CommentNew.jsx

#### 회원 기능
* lion-board/src/pages/user 폴더 생성후 sample 폴더 html 파일의 `<main>` 태그 복사해서 완성
  - Login.jsx
  - Signup.jsx

#### 에러 페이지
* sample/error.html 참고해서 lion-board/src/pages/ErrorPage.jsx 파일 작성
  ```jsx
  import Footer from "@components/Footer";
  import Header from "@components/Header";

  export default function ErrorPage() {
    return (
      <div className="flex flex-col min-h-screen dark:bg-gray-700 dark:text-gray-200 transition-color duration-500 ease-in-out">
        <Header />
        <div className="py-20 bg-red-100 border border-red-400 text-red-700 p-4 rounded-lg flex flex-col items-center space-y-2">
          <h2 className="text-xl font-semibold mb-2 text-center">🚧 앗, 무언가 잘못됐네요!</h2>
          <h3 className="text-md font-semibold mb-2 text-center">존재하지 않는 페이지입니다.</h3>
          <p className="pt-12 text-center">이 오류는 더 나은 서비스를 위한 첫걸음이에요. 조금만 기다려 주세요!</p>
          <button className="bg-red-600 text-white py-2 px-4 rounded hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-red-600">
            ⚙️ 다시 시도
          </button>
        </div>
        <Footer />
      </div>
    );
  }
  ```

### 라우터 작성
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch05-router#5%EC%9E%A5-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EB%9D%BC%EC%9A%B0%ED%84%B0
* lion-board/src/routes.jsx 파일 생성
* BrowserRouter 사용
  ```jsx
  import Layout from "@components/layout";
  import Detail from "@pages/board/Detail";
  import Edit from "@pages/board/Edit";
  import List from "@pages/board/List";
  import New from "@pages/board/New";
  import ErrorPage from "@pages/ErrorPage";
  import MainPage from "@pages/index";
  import Login from "@pages/user/Login";
  import Signup from "@pages/user/Signup";

  import { createBrowserRouter } from "react-router-dom";

  const router = createBrowserRouter([
    {
      path: "/",
      errorElement: <ErrorPage />,
      element: <Layout />,
      children: [
        { index: true, element: <MainPage /> },
        { path: ":type", element: <List /> },
        { path: ":type/new", element: <New /> },
        { path: ":type/:_id", element: <Detail /> },
        { path: ":type/:_id/edit", element: <Edit /> },
        { path: "users/login", element: <Login /> },
        { path: "users/signup", element: <Signup /> },
      ]
    },
  ]);

  export default router;
  ```

### App.jsx 수정
* 리액트 라우터 추가
  ```jsx
  import { RouterProvider } from "react-router-dom";
  import router from "@/routes";

  function App() {
    return (
      <RouterProvider router={ router } />
    );
  }

  export default App;
  ```

### 링크 확인
* 링크가 제대로 동작하지 않는 부분 수정
  - a 태그 대신 Link로 수정
  - href 속성을 to로 수정
  - 링크를 라우터에 등록한 URL로 수정
* Header.jsx 예시
  ```jsx
  <Link to="/info">정보공유</Link>
  <Link to="/free">자유게시판</Link>
  <Link to="/qna">질문게시판</Link>
  ```

# 2단계
* API 서버 호출
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch09-ajax#axios-%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC

## React Query 설정
* QueryClient 지정
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch09-ajax#%EC%82%AC%EC%9A%A9-%EC%84%A4%EC%A0%95
* main.jsx
  ```jsx
  import { StrictMode } from 'react'
  import { createRoot } from 'react-dom/client'
  import './index.css';
  import App from './App.jsx'
  import { QueryClient, QueryClientProvider } from '@tanstack/react-query';
  import { ReactQueryDevtools } from '@tanstack/react-query-devtools';

  const client = new QueryClient();

  createRoot(document.getElementById('root')).render(
    <StrictMode>
      <QueryClientProvider client={ client }>
        <App />
        <ReactQueryDevtools initialIsOpen={false} />
      </QueryClientProvider>
    </StrictMode>,
  )
  ```

## axios 커스텀 훅 작성
* hooks/useAxiosInstance.jsx 작성
  ```jsx
  import axios from "axios";

  function useAxiosInstance() {
    const instance = axios.create({
      baseURL: 'https://11.fesp.shop',
      timeout: 1000*15,
      headers: {
        'Content-Type': 'application/json',
        accept: 'application/json',
        'client-id': '00-board',
      }
    });

    instance.interceptors.request.use((config) => {
      config.params = {
        delay: 500,
        ...config.params,
      };
      return config;
    });

    instance.interceptors.response.use((response) => {

      return response;
    }, (error) => {

      return Promise.reject(error);
    });

    return instance;
  }

  export default useAxiosInstance;
  ```

## API 서버 호출
### 게시물 목록 조회
* pages/List.jsx
  ```jsx
  ...
  export default function List() {
    const axios = useAxiosInstance();
    const { type } = useParams();

    const { data } = useQuery({
      queryKey: ['posts', type],
      queryFn: () => axios.get('/posts', { params: { type } }),
      select: res => res.data,
      staleTime: 1000*10,
    });

    if(!data){
      return <div>로딩중...</div>;
    }

    const list = data.item.map(item => <ListItem key={item._id} item={ item } />);

    return (
      ...
      <tbody>{ list }</tbody>
      ...
    );
  }
  ```

* pages/board/ListItem.jsx
  ```jsx
  import PropTypes from "prop-types";
  import { Link } from "react-router-dom";

  ListItem.propTypes = {
    item: PropTypes.shape({
      _id: PropTypes.number.isRequired,
      title: PropTypes.string.isRequired,
      writer: PropTypes.shape({
        name: PropTypes.string.isRequired,
      }),
      views: PropTypes.number,
      repliesCount: PropTypes.number,
      createdAt: PropTypes.string.isRequired,
    }).isRequired,

  };
  export default function ListItem({ item }) {
    return (
      <>
        <tr className="border-b border-gray-200 hover:bg-gray-200 dark:hover:bg-gray-700 transition duration-300 ease-in-out">
          <td className="p-2 text-center">{ item._id }</td>
          <td className="p-2 truncate indent-4"><Link to={`${ item._id }`} className="cursor-pointer">{ item.title }</Link></td>
          <td className="p-2 text-center truncate">{ item.user.name }</td>
          <td className="p-2 text-center hidden sm:table-cell">{ item.views }</td>
          <td className="p-2 text-center hidden sm:table-cell">{ item.repliesCount }</td>
          <td className="p-2 truncate text-center hidden sm:table-cell">{ item.createdAt }</td>
        </tr>
      </>
    );
  }
  ```

### 게시물 등록
#### 입력값 검증 실패 메세지 출력용 컴포넌트 작성
* components/InputError.jsx 생성
  ```jsx
  export default function InputError({ target }){
    if(!target) return;
    return (
      <p className="ml-2 mt-1 text-sm text-red-500 dark:text-red-400">
        { target.message }
      </p>
    );
  }
  ```

#### 폼 데이터 관리
* pages/board/New.jsx
  - react-hook-form 사용
    ```jsx
    import { useForm } from 'react-hook-form';
    ...
    export default function New() {
      const { register, handleSubmit, formState: { errors } } = useForm();
      ...
    }
    ```

  - react-hook-form의 register 함수로 입력 요소 등록 및 검증 실패 메세지 출력
    ```jsx
    <input
      id="title"
      type="text"
      placeholder="제목을 입력하세요." 
      className="w-full py-2 px-4 border rounded-md dark:bg-gray-700 border-gray-300 focus:outline-none focus:border-blue-500 focus:ring-2 focus:ring-blue-200"
      { ...register('title', { required: '제목을 입력하세요.' }) }
    />
    <InputError target={ errors.title } />
    ```

    ```jsx
    <textarea 
      id="content"
      rows="15" 
      placeholder="내용을 입력하세요."
      className="w-full p-4 text-sm border rounded-lg border-gray-300 bg-gray-50 focus:outline-none focus:border-blue-500 focus:ring-2 focus:ring-blue-200 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white"
      { ...register('content', { required: '내용을 입력하세요' }) }
    />
    <InputError target={ errors.content } />
    ```

#### 게시물 등록 API 호출
* pages/board/New.jsx
* API 호출
  ```jsx
  const { type } = useParams();
  const navigate = useNavigate();
  const axios = useAxiosInstance();

  const queryClient = useQueryClient();

  const addItem = useMutation({
    mutationFn: formData => {
      formData.type = type;
      return axios.post('/posts', formData);
    },
    onSuccess: () => {
      alert('새로운 글이 등록 되었습니다.');
      queryClient.invalidateQueries({ queryKey: ['posts', type] });
      navigate(`/${type}`);
    },
  });
  ```

* form의 onSubmit 이벤트 등록
  ```jsx
  <form onSubmit={ handleSubmit(addItem.mutate) }>
  ```

### 게시물 상세 보기
* pages/board/Detail.jsx
  ```jsx
  import useAxiosInstance from "@hooks/useAxiosInstance";
  import CommentList from "@pages/board/CommentList";
  import { useQuery } from "@tanstack/react-query";
  import { Link, useParams } from "react-router-dom";

  export default function Detail() {

    const { type, _id } = useParams();
    const axios = useAxiosInstance();

    const { data } = useQuery({
      queryKey: ['posts', _id],
      queryFn: () => axios.get(`/posts/${_id}`),
      select: res => res.data,
      staleTime: 1000*10,
    });

    if(!data){
      return <div>로딩중...</div>;
    }
    
    return (
      <>
        <main className="container mx-auto mt-4 px-4">

          <section className="mb-8 p-4">
            <form action="/info">
            <div className="font-semibold text-xl">제목 : { data.item.title }</div>
              <div className="text-right text-gray-400">작성자 : { data.item.user.name }</div>
              <div className="mb-4">
                <div>
                  <pre className="font-roboto w-full p-2 whitespace-pre-wrap">{ data.item.content }</pre>
                </div>
                <hr/>
              </div>
              <div className="flex justify-end my-4">
                <Link to={`/${type}`} className="bg-orange-500 py-1 px-4 text-base text-white font-semibold ml-2 hover:bg-amber-400 rounded">목록</Link>
                <Link to={`/${type}/${_id}/edit`} className="bg-gray-900 py-1 px-4 text-base text-white font-semibold ml-2 hover:bg-amber-400 rounded">수정</Link>
                <button type="submit" className="bg-red-500 py-1 px-4 text-base text-white font-semibold ml-2 hover:bg-amber-400 rounded">삭제</button>
              </div>
            </form>
          </section>

          <CommentList data={ data.item.replies } />

          </main>
      </>
    );
  }
  ```

#### 댓글 목록 조회
* pages/board/CommentList.jsx
  ```jsx
  import CommentListItem from "@pages/board/CommentListItem";
  import CommentNew from "@pages/board/CommentNew";
  import PropTypes from "prop-types";

  CommentList.propTypes = {
    data: PropTypes.array.isRequired,
  };

  export default function CommentList({ data=[] }) {

    const list = data.map(item => <CommentListItem key={ item._id } item={ item } />);
    return (
      <>
        <section className="mb-8">
          <h4 className="mt-8 mb-4 ml-2">댓글 { data.length }개</h4>

          { list }

          <CommentNew />

        </section>
      </>
    );
  }
  ```

* pages/board/CommentListItem.jsx
  ```jsx
  import PropTypes from "prop-types";
  import { Link } from "react-router-dom";

  CommentListItem.propTypes = {
    item: PropTypes.shape({
      _id: PropTypes.number.isRequired,
      user: PropTypes.shape({
        _id: PropTypes.number,
        name: PropTypes.string,
        image: PropTypes.string,
      }),
      content: PropTypes.string.isRequired,
      createdAt: PropTypes.string.isRequired,
    }).isRequired,
  };
  export default function CommentListItem({ item }) {
    return (
      <>
        <div className="shadow-md rounded-lg p-4 mb-4">
          <div className="flex justify-between items-center mb-2">
            { item.user.image && 
              <img
                className="w-8 mr-2 rounded-full"
                src={`https://11.fesp.shop${ item.user.image }`}
                alt={`${ item.user.name } 프로필 이미지`}
              />
            }
            <Link to="" className="text-orange-400">${ item.user.name }</Link>
            <time className="ml-auto text-gray-500" dateTime={ item.user.createdAt }>{ item.user.createdAt }</time>
          </div>
          <div className="flex justify-between items-center mb-2">
            <form action="#">
              <pre className="whitespace-pre-wrap text-sm">{ item.content }</pre>
              <button type="submit" className="bg-red-500 py-1 px-2 text-sm text-white font-semibold ml-2 hover:bg-amber-400 rounded">삭제</button>
            </form>
          </div>  
        </div>
      </>
    );
  }
  ```

### 회원 가입
* 남은 기능인 게시물 수정, 게시물 삭제, 댓글 등록, 댓글 삭제는 로그인 한 회원만 사용 가능한 기능

* pages/user/Signup.jsx
  - react-hook-form으로 입력 요소 관리
    ```jsx
    const { register, handleSubmit, formState: { errors }, setError } = useForm();
    ```

    ```jsx
    <input
      type="text"
      id="name"
      placeholder="이름을 입력하세요"
      className="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-orange-400 dark:bg-gray-700"
      { ...register('name', { required: '이름은 필수입니다.' }) }
    />
    <InputError target={ errors.name } />
    ```

    ```jsx
    <input
      type="email"
      id="email"
      placeholder="이메일을 입력하세요"
      className="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-orange-400 dark:bg-gray-700"
      { ...register('email', { required: '이메일은 필수입니다.' }) }
    />
    <InputError target={ errors.email } />
    ```

    ```jsx
    <input
      type="password"
      id="password"
      placeholder="비밀번호를 입력하세요"
      className="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-orange-400 dark:bg-gray-700"
      { ...register('password', { required: '비밀번호는 필수입니다.' }) }
    />
    <InputError target={ errors.password } />
    ```

    ```jsx
    <input
      type="file"
      id="attach"
      accept="image/*"
      placeholder="이미지를 선택하세요"
      className="w-full px-3 py-2 border rounded-lg dark:bg-gray-700"
      { ...register('attach') }
    />
    ```

  - 회원 가입 이벤트 추가
  ```jsx
  const navigate = useNavigate();
  const axios = useAxiosInstance();
  const addUser = useMutation({
    mutationFn: userInfo => {
      userInfo.type = 'user';
      return axios.post(`/users`, userInfo);
    },
    onSuccess: () => {
      alert('회원가입 완료');
      navigate(`/`);
    },
    onError: (err) => {
      console.error(err);
      if(err.response?.data.errors){
        err.response?.data.errors.forEach(error => setError(error.path, { message: error.msg }));
      }else{
        alert(err.response?.data.message || '잠시후 다시 요청하세요.');
      }
    },
  });
  ```

  - form에 submit 이벤트 추가
    ```jsx
    <form onSubmit={ handleSubmit(addUser.mutate) }>
    ```

### 로그인
* pages/user/Login.jsx
  - react-hook-form으로 입력 요소 관리
    ```jsx
    const { register, handleSubmit, formState: { errors }, setError } = useForm();
    ```

    ```jsx
    <input
      id="email"
      type="email"
      placeholder="이메일을 입력하세요"
      className="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-orange-400 dark:bg-gray-700"
      { ...register('email', { required: '이메일은 필수입니다.' }) }
    />
    <InputError target={ errors.email } />
    ```

    ```jsx
    <input
      id="password"
      type="password"
      placeholder="비밀번호를 입력하세요"
      className="w-full px-3 py-2 border rounded-lg focus:outline-none focus:border-orange-400 dark:bg-gray-700"
      { ...register('password', { required: '비밀번호는 필수입니다.' }) }
    />
    <InputError target={ errors.password } />
    ```

  - 로그인 이벤트 추가
  ```jsx
  const navigate = useNavigate();
  const axios = useAxiosInstance();
  const login = useMutation({
    mutationFn: formData => axios.post(`/users/login`, formData),
    onSuccess: (res) => {
      alert(res.data.item.name + '님, 로그인 되었습니다.');
      navigate(`/`);
    },
    onError: (err) => {
      if(err.response?.data.errors){
        err.response?.data.errors.forEach(error => setError(error.path, { message: error.msg }));
      }else{
        alert(err.response?.data.message || '잠시후 다시 요청하세요.');
      }
    },
  });
  ```

  - form에 submit 이벤트 추가
    ```jsx
    <form onSubmit={ handleSubmit(login.mutate) }>
    ```

### 요청 헤더에 Authorization 추가
* hooks/useAxiosInstance.jsx 수정
  - 임시로 access token 지정
  - [Access Token]은 로그인 완료후 받은 응답 데이터의 accessToken값 사용
  ```js
  config.headers.Authorization = `Bearer [access Token]`;
  ```

### 게시물 수정
* Edit.jsx 수정
  - 게시물 상세 정보 조회
    ```jsx
    const { type, _id } = useParams();
    const axios = useAxiosInstance();

    const { data } = useQuery({
      queryKey: ['posts', _id],
      queryFn: () => axios.get(`/posts/${_id}`),
      select: res => res.data,
      staleTime: 1000*10,
    });
    ```

  - react-hook-form으로 form 요소 관리
    ```jsx
      const { register, handleSubmit, formState: { errors } } = useForm({
      defaultValues: {
        title: data?.item.title,
        content: data?.item.content,
      }
    });
    ```

    ```jsx
    <input
      id="title"
      type="text"
      placeholder="제목을 입력하세요." 
      className="w-full py-2 px-4 border rounded-md dark:bg-gray-700 border-gray-300 focus:outline-none focus:border-blue-500 focus:ring-2 focus:ring-blue-200"
      { ...register('title', { required: '제목은 필수입니다.' }) }
    />
    <InputError target={ errors.title } />
    ```

    ```jsx
    <textarea 
      id="content"
      rows="15" 
      placeholder="내용을 입력하세요."
      className="w-full p-4 text-sm border rounded-lg border-gray-300 bg-gray-50 focus:outline-none focus:border-blue-500 focus:ring-2 focus:ring-blue-200 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white"
      { ...register('content', { required: '내용은 필수입니다.' }) }
    />
    <InputError target={ errors.content } />
    ```
    
  - submit 이벤트 추가
    ```jsx
    <form onSubmit={ handleSubmit(onSubmit) }>
    ```

    ```jsx
    const navigate = useNavigate();
    const queryClient = useQueryClient();
    const updateItem = useMutation({
      mutationFn: formData => axios.patch(`/posts/${_id}`, formData),
      onSuccess: () => {
        alert('게시물이 수정되었습니다.');
        queryClient.invalidateQueries({ queryKey: ['posts', _id] });
        navigate(`/${type}/${_id}`);
      }
    });
    ```

### 게시물 삭제
* pages/board/Detail.jsx 수정
  - 삭제 이벤트 추가
    ```jsx
    const navigate = useNavigate();
    const queryClient = useQueryClient();
    const removeItem = useMutation({
      mutationFn: () => axios.delete(`/posts/${_id}`),
      onSuccess: () => {
        alert('게시글이 삭제 되었습니다.');
        queryClient.invalidateQueries({ queryKey: ['posts', type] });
        navigate(`/${type}`);
      }
    });

    const onSubmit = (event) => {
      event.preventDefault();
      removeItem.mutate();
    };
    ```

    - form의 submit 이벤트 추가
    ```jsx
    <form onSubmit={ onSubmit }>
    ```

### 댓글 등록
* pages/board/CommentNew.jsx
  ```jsx
  import InputError from "@components/InputError";
  import useAxiosInstance from "@hooks/useAxiosInstance";
  import { useMutation, useQueryClient } from "@tanstack/react-query";
  import { useForm } from "react-hook-form";
  import { useNavigate, useParams } from "react-router-dom";

  export default function CommentNew() {
    const { type, _id } = useParams();
    const navigate = useNavigate();

    const { register, handleSubmit, formState: { errors } } = useForm();
    const axios = useAxiosInstance();

    const queryClient = useQueryClient();

    const addItem = useMutation({
      mutationFn: formData => {
        return axios.post(`/posts/${_id}/replies`, formData);
      },
      onSuccess: () => {
        alert('댓글이 등록 되었습니다.');
        queryClient.invalidateQueries({ queryKey: ['posts', _id] });
        navigate(`/${type}/${_id}`);
      },
    });
    return (
      <>
        <div className="p-4 border border-gray-200 rounded-lg">
          <h4 className="mb-4">새로운 댓글을 추가하세요.</h4>
          <form onSubmit={ handleSubmit(addItem.mutate) }>
            <input type="hidden" { ...register('name') } value={'무지'} />
            <div className="mb-4">
              <textarea
                rows="3"
                cols="40"
                className="block p-2 w-full text-sm border rounded-lg border-gray-300 bg-gray-50 focus:outline-none focus:border-blue-500 focus:ring-2 focus:ring-blue-200 dark:bg-gray-700 dark:border-gray-600 dark:placeholder-gray-400 dark:text-white"
                placeholder="내용을 입력하세요."
                { ...register('content', { required: '내용을 입력하세요.' }) }
              />
              <InputError target={ errors.content } />
              
            </div>
            <button type="submit" className="bg-orange-500 py-1 px-4 text-sm text-white font-semibold ml-2 hover:bg-amber-400 rounded">댓글 등록</button>
          </form>
        </div>
      </>
    );
  }
  ```

### 댓글 삭제
* pages/board/CommentListItem.jsx
  - 삭제 이벤트 처리
    ```jsx
    const { _id } = useParams();
    const axios = useAxiosInstance();
    const queryClient = useQueryClient();
    const removeItem = useMutation({
      mutationFn: () => axios.delete(`/posts/${_id}/replies/${item._id}`),
      onSuccess: () => {
        queryClient.invalidateQueries({ queryKey: ['posts', _id] });
      }
    });

    const onSubmit = (event) => {
      event.preventDefault();
      removeItem.mutate();
    };
    ```

  - form에 삭제 이벤트 등록
    ```jsx
    <form onSubmit={ onSubmit }>
    ```

### 회원 가입시 프로필 이미지 추가
* Signup.jsx
  - mutationFn 수정
    ```jsx
    mutationFn: async userInfo => {
      // 이미지 먼저 업로드
      if(userInfo.attach.length > 0){
        const imageFormData = new FormData();
        imageFormData.append('attach', userInfo.attach[0]);

        const fileRes = await axios.post('/files', imageFormData, {
          headers: {
            'Content-Type': 'multipart/form-data'
          }
        });

        userInfo.image = fileRes.data.item[0].path;
        delete userInfo.attach;
      }

      userInfo.type = 'user';
      return axios.post(`/users`, userInfo);
    },
    onSuccess: () => {
      alert('회원가입 완료');
      navigate(`/`);
    },
    onError: (err) => {
      console.error(err);
      if(err.response?.data.errors){
        err.response?.data.errors.forEach(error => setError(error.path, { message: error.msg }));
      }else{
        alert(err.response?.data.message || '잠시후 다시 요청하세요.');
      }
    },
    ```

# 3단계
* 전역 상태 관리
  - 로그인과 JWT 토큰 관리
  - 테마 적용

## 로그인과 JWT 토큰 관리
* 로그인 완료 후에 전달되는 토큰을 전역 상태관리로 저장
* 로그인 여부에 따른 조건부 렌더링
  - 환영 메세지 vs. 로그인 버튼
  - 내가 작성한 게시글만 수정, 삭제 버튼 노출
* Authorization 요청 헤더에 access token 추가

### Zustand store 설정
* src/zustand/userStore.js 생성
  ```js
  import { create } from "zustand";
  import { persist, createJSONStorage } from 'zustand/middleware';

  const UserStore = (set) => ({
    user: null,
    setUser: (user) => set({ user }),
    resetUser: () => set({ user: null }),
  });

  // 스토리지를 사용하지 않을 경우
  // const useUserStore = create(UserStore);

  // 스토리지를 사용할 경우
  const useUserStore = create(persist(UserStore, {
    name: 'userStore',
    storage: createJSONStorage(() => sessionStorage) // 기본은 localStorage
  }));

  export default useUserStore;
  ```

### 로그인 완료 후 토큰 저장
* Login.jsx
  - Zustand store의 setUser 꺼내기
    ```jsx
    const setUser = useUserStore(store => store.setUser);
    ```

  - 로그인 성공 후 호출되는 onSuccess에서 setUser 호출
    ```jsx
    onSuccess: (res) => {
      const user = res.data.item;
      setUser({
        _id: user._id,
        name: user.name,
        profile: user.image,
        accessToken: user.token.accessToken,
        refreshToken: user.token.refreshToken,
      });
      alert(res.data.item.name + '님, 로그인 되었습니다.');
      navigate(`/`);
    }
    ```

### axios 요청 헤더에 Authorization 추가
* useAxiosInstance.js
  - user 꺼내기
    ```js
    function useAxiosInstance() {
      const { user } = useUserStore();
      ......
    }
    ```

  - axios 요청 인터셉터에 추가
    ```js
    instance.interceptors.request.use((config) => {
      ......
      if(user){
        let token = user.accessToken;
        config.headers.Authorization = `Bearer ${ token }`;
      }
      ......
    });
    ```

### 로그인 상태에 따른 조건부 렌더링
* 로그인된 사용자에게는 사용자 정보를 보여주고 로그인되지 않은 사용자에게는 로그인 버튼과 회원가입 버튼을 보여줌
* Header.jsx
  - user 꺼내기
    ```js
    export default function Header() {
      const { user } = useUserStore();
      ......
    }
    ```
  - sample/info/2/index.html의 `<header>` 영역의 `<form>` 태그 복사해서 작성
    ```jsx
    { user ? (
      <form action="/">
        <p className="flex items-center">
          { user.profile && (
            <img 
              className="w-8 rounded-full mr-2" 
              src={`https://11.fesp.shop${user.profile}`}
              width="40" 
              height="40" 
              alt="프로필 이미지"
            />
          ) }
          { user.name }님
          <button type="submit" className="bg-gray-900 py-1 px-2 text-sm text-white font-semibold ml-2 hover:bg-amber-400 rounded">로그아웃</button>
        </p>
      </form>
    ) : (
      <div className="flex justify-end">
        <Link to="/users/login" className="bg-orange-500 py-1 px-2 text-sm text-white font-semibold ml-2 hover:bg-amber-400 rounded">로그인</Link>
        <Link to="/users/signup" className="bg-gray-900 py-1 px-2 text-sm text-white font-semibold ml-2 hover:bg-amber-400 rounded">회원가입</Link>
      </div>
    ) }
    ```

  - 로그아웃 기능 추가
    ```jsx
    export default function Header() {
      const { user, resetUser } = useUserStore();
      const handleLogout = (event) => {
        event.preventDefault();
        resetUser();
      };
    }
    ```

  - form에 submit 이벤트 추가
    ```jsx
    <form onSubmit={ handleLogout }>
    ```
* List.jsx
  - 로그인 된 사용자만 게시글 작성 가능
  ```jsx
  const { user } = useUserStore();
  ```

  ```jsx
  { user && 
    <Link to={`/${type}/new`} className="......">글작성</Link>
  }
  ```

* Detail.jsx
  - 본인의 글에 대해서 수정, 삭제 버튼 노출
  - 로그인 정보 꺼내기
    ```jsx
    export default function Detail() {
      const { user } = useUserStore();
      ......
    }
    ```

  - 조건부 렌더링
    ```jsx
    { user?._id === data.item.user._id && (
      <>
        <Link to={`/${type}/${_id}/edit`} className="bg-gray-900 py-1 px-4 text-base text-white font-semibold ml-2 hover:bg-amber-400 rounded">수정</Link>
        <button type="submit" className="bg-red-500 py-1 px-4 text-base text-white font-semibold ml-2 hover:bg-amber-400 rounded">삭제</button>
      </>
    ) }
    ```

  - 내가 작성한 글 상세조회 화면에서 로그아웃하면 수정, 삭제 버튼도 사라짐

* CommentListItem.jsx
  - 로그인 한 사용자에게만 자신의 댓글 삭제 버튼 추가
    ```jsx
    { user?._id === item.user._id &&  <button type="submit" className="bg-red-500 py-1 px-2 text-sm text-white font-semibold ml-2 hover:bg-amber-400 rounded">삭제</button> }
    ```

### access token 만료시 처리
* access token이 만료되면 refresh token을 이용해서 access token을 재발급

#### axios 인터셉터 설정
* useAxiosInstance.js
  ```jsx
  import useUserStore from "@zustand/userStore";
  import axios from "axios";
  import { useLocation, useNavigate } from "react-router-dom";

  // access token 재발급 URL
  const REFRESH_URL = '/auth/refresh';

  function useAxiosInstance() {
    const { user, setUser } = useUserStore();

    const navigate = useNavigate();
    const location = useLocation();
    
    const instance = axios.create({
      baseURL: 'https://11.fesp.shop',
      timeout: 1000*15,
      headers: {
        'Content-Type': 'application/json', // request의 데이터 타입
        accept: 'application/json', // response의 데이터 타입
        'client-id': '00-board',
      }
    });

    // 요청 인터셉터 추가하기
    instance.interceptors.request.use((config) => {
      // refresh 요청일 경우 Authorization 헤더는 이미 refresh token으로 지정되어 있음
      if(user && config.url !== REFRESH_URL){
        config.headers.Authorization = `Bearer ${ user.accessToken }`;
      }
      
      // 요청이 전달되기 전에 필요한 공통 작업 수행
      config.params = {
        delay: 500,
        ...config.params, // 기존 쿼리스트링 복사
      };
      return config;
    });

    // 응답 인터셉터 추가하기
    instance.interceptors.response.use((response) => {
      // 2xx 범위에 있는 상태 코드는 이 함수가 호출됨
      // 응답 데이터를 이용해서 필요한 공통 작업 수행

      return response;
    }, async (error) => {
      // 2xx 외의 범위에 있는 상태 코드는 이 함수가 호출됨
      // 공통 에러 처리
      console.error('인터셉터', error);
      const { config, response } = error;

      if(response?.status === 401){ // 인증 실패
        if(config.url === REFRESH_URL){ // refresh token 만료
          navigateLogin();
        }else if(user){ // 로그인 했으나 access token 만료된 경우
          // refresh 토큰으로 access 토큰 재발급 요청
          const { data: { accessToken } } = await instance.get(REFRESH_URL, {
            headers: {
              Authorization: `Bearer ${user.refreshToken}`
            }
          });
          setUser({ ...user, accessToken });
          // 갱신된 accessToken으로 재요청
          config.headers.Authorization = `Bearer ${ accessToken }`;        
          return axios(config);
        }else{ // 로그인 안한 경우
          navigateLogin();
        }
      }
      return Promise.reject(error);
    });

    function navigateLogin(){
      const gotoLogin = confirm('로그인 후 이용 가능합니다.\n로그인 페이지로 이동하시겠습니까?');
      gotoLogin && navigate('/users/login', { state: { from: location.pathname } });
    }

    return instance;
  }

  export default useAxiosInstance;
  ```

#### 로그인 후 페이지 이동
* 로그인 하지 않은 상태로 인증이 필요한 API를 호출하면 로그인 페이지로 이동하는데 로그인을 완료한 후 이전 페이지로 이동하도록 구현
* Login.jsx
  ```jsx
  const login = useMutation({
    const location = useLocation();
    ......
    onSuccess: (res) => {
      ......
      alert(res.data.item.name + '님, 로그인 되었습니다.');
      navigate(location.state?.from || `/`);
    },
  });
  ```

## 다크 모드 적용
### Zustand store 설정
* zustand/themeStore.js 생성
  ```js
  import { create } from "zustand";
  import { persist } from 'zustand/middleware';

  const ThemeStore = (set) => ({
    isDarkMode: window.matchMedia('(prefers-color-scheme: dark)').matches ? true : false,
    toggleTheme: () => set((state) => ({ isDarkMode: !state.isDarkMode })),
  });

  const useThemeStore = create(persist(ThemeStore, {
    name: 'themeStore'
  }));

  export default useThemeStore;
  ```

### ThemeButton 컴포넌트 작성
* components/ThemeButton.jsx 작성
  - Header.jsx 에서 `<button>` 코드 복사
  - 현재 설정된 모드에 따라 sun, moon 이미지 hidden
  - onClick 이벤트 추가
    ```jsx
    import useThemeStore from "@zustand/themeStore";

    export default function ThemeButton() {
      const { isDarkMode, toggleTheme } = useThemeStore();

      const sun = isDarkMode ? '' : 'hidden';
      const moon = isDarkMode ? 'hidden' : '';
      
      console.log('isDarkMode', isDarkMode);

      return (
        <button
          type="button"
          data-toggle-dark="dark"
          onClick={ toggleTheme }
          className="......"
        >
          <svg
            data-toggle-icon="moon"
            className={`w-3.5 h-3.5 ${moon}`}
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            fill="currentColor"
            viewBox="0 0 18 20"
          >
            <path d="......"></path>
          </svg>
          <svg
            data-toggle-icon="sun"
            className={`w-3.5 h-3.5 ${sun}`}
            aria-hidden="true"
            xmlns="http://www.w3.org/2000/svg"
            fill="currentColor"
            viewBox="0 0 20 20"
          >
            <path d="......"></path>
          </svg>
          <span className="sr-only">Toggle dark/light mode</span>
        </button>
      );
    }
    ```

* Header.jsx
  - `<ThemeButton>` 적용
    ```jsx
    <header>
      <nav>      
        ......
        <div>
          { user ? (
            ......
          ) : (
            ......
          ) }

          <ThemeButton />

        </div>
      </nav>
    </header>
    ```

### Tailwind CSS에 적용
* 참고: https://tailwindcss.com/docs/dark-mode
* 클래스명에 접두사로 `dark:`를 붙이면 다크 모드에서 적용되는 스타일을 지정할 수 있음
* 예시
  ```jsx
  <header className="px-8 min-w-80 bg-slate-100 dark:bg-gray-600 text-gray-800 dark:text-gray-200 transition-color duration-500 ease-in-out">
  ```

#### Tailwind CSS의 다크 모드 전략

##### media 전략
- 기본값
- CSS 미디어 기능 `prefers-color-scheme`를 이용해서 운영체제 설정을 따름

##### selector 전략
- 운영체제 설정에 의존하지 않고 수동으로 다크 모드 전환
- `dark` 클래스가 적용되어 있는 요소의 모든 하위 요소에는 일반 클래스 대신 `dark:` 접두사가 붙어있는 클래스가 적용됨
  - `<header>` 예시
    + `bg-slate-100` 대신 `dark:bg-gray-600` 적용
    + `text-gray-800` 대신 `dark:text-gray-200` 적용
- tailwind.config.js 설정에 추가
  ```js
  export default {
    ......
    darkMode: "selector",
  }
  ```

### 루트 엘리먼트에 dark 클래스 설정
* App.jsx
  ```jsx
  function App() {
    const { isDarkMode } = useThemeStore();
    if(isDarkMode){
      document.documentElement.classList.add('dark');
    }else{
      document.documentElement.classList.remove('dark');
    }
    ......
  }
  ```

# 4단계
* 배포
* 최적화
  - SEO
  - 사용자 경험(UX) 최적화
    + Lazy loading
    + 로딩중 상태 표시(`<Suspense>` 사용)
    + alert 대신 toast 사용
    + NavLink 사용
    + 에러 처리

## 배포 준비
### 배포전 테스트
```sh
npm run build
npm run preview
```

### .env
* Vite 환경 변수와 모드: <https://ko.vitejs.dev/guide/env-and-mode.html>
* dotenv: DB 접속 정보, API KEY 등의 환경설정 정보를 코드에 직접 작성하지 않고 외부 파일로 만들어서 관리하기 위해 사용하는 nodejs 확장모듈
* 개발, 테스트, 운영 등의 여러 환경에서 각각 다른 값을 사용해야 하는 경우 각 환경 설정 정보를 가진 파일을 따로 작성해서 적용 가능
* OS의 환경변수로 추가됨

#### 설치
```sh
npm i dotenv
```
* CRA나 Vite로 프로젝트를 생성했을 경우 따로 설치할 필요 없음

#### 환경 설정 파일
* 프로젝트 루트에 .env 파일 생성
  - CRA로 프로젝트를 생성했을 경우 환경변수는 반드시 REACT_APP_ 접두사로 시작해야 함
  - Vite로 포로젝트를 생성했을 경우 환경변수는 반드시 VITE_ 접두사로 시작해야 함
* 예시
  ```
  VITE_API_SERVER=https://11.fesp.shop
  VITE_KAKAO_MAP_API_KEY=acd4396a562ece1a9a522481df8561c5
  VITE_KAKAO_LOGIN_KEY=cbaac2342342342348749845670acbb
  ```

* .env 파일을 수정한 후에는 서버 재시작 필요
* 외부에 노출되면 안되는 중요한 정보를 담고 있을 경우 github의 공개 프로젝트라면 .gitignore에 추가해서 커밋되지 않도록 지정
  ```
  *.env*
  ```

* 이미 .env 파일이 git에서 추적중인 상태라서 추가한 ignore가 적용 안되다면 캐시 삭제
  - 기존의 모든 변경사항은 커밋 후 다음 명령 실행
    ```sh
    git rm -r --cached .
    git add .
    git commit -m "gitignore 다시 적용"
    ```

#### 컴포넌트에서 사용(*.js, *.jsx, *.ts, *.tsx)
* CRA 프로젝트는 process.env 변수를 통해서 사용
* Vite 프로젝트는 import.meta.env 변수를 통해서 사용
* 예시
```js
const res = await axios.get(import.meta.env.VITE_API_SERVER + '/products');
const res = await axios.get(`${import.meta.env.VITE_API_SERVER}/products`);
```

#### HTML에서 사용(*.html)
* %% 사이에 환경변수 지정
* 예시
```html
<script type="text/javascript" src="https://dapi.kakao.com/v2/maps/sdk.js?appkey=%VITE_KAKAO_MAP_API_KEY%"></script>
```

#### 환경별 파일 분리
* CRA로 프로젝트를 생성했을 경우 react-scripts 실행 명령에 따라서 각 환경에 맞는 .env 파일이 추가적으로 적용됨
* Vite로 프로젝트를 생성했을 경우 vite 실행 명령에 따라서 각 환경에 맞는 .env 파일이 추가적으로 적용됨
  - npm run dev
    + .env.development 설정이 추가 적용
  - npm run build
    + .env.production 설정이 추가 적용
* 다른 .env 파일과 중복된 환경변수가 있을 경우 이전 환경변수 값을 재정의함

##### 모든 환경
* .env
  - 모든 환경에서 적용되는 기본 설정 파일

##### 개발 환경
* .env.development
  - .env 파일의 환경변수를 재정의
  - npm run dev로 개발 서버 구동시 적용

##### 운영 환경
* .env.production
  - .env 파일의 환경변수를 재정의
  - npm run build로 번들링 할때 적용

##### 로컬 환경(개발자 PC)
* 개발서버, 테스트서버, 운영서버에는 만들 필요 없고 개발자 개인 PC에만 만들어서 적용시키는 파일
* 로컬에 DB나 API 서버등을 직접 구축해서 사용할 경우에 필요
* .env.local
  - 로컬에 개발, 운영 환경이 구분없이 구축되어 있거나 두 환경에서 공통으로 사용할 환경 변수 정의
  - .env, .env.development, .env.production 파일의 환경변수를 재정의
* .env.development.local
  - 로컬에 개발, 운영 환경을 분리해서 구축했을 경우 로컬 개발 환경에서 사용
  - .env, .env.development, .env.local 파일의 환경변수를 재정의
* .env.production.local
  - 로컬에 개발, 운영 환경을 분리해서 구축했을 경우 로컬 운영 환경에서 사용
  - .env, .env.production, .env.local 파일의 환경변수를 재정의

#### .env 파일 적용 우선순위
* .env 파일이 적용되는 순서
* 좌측에서 우측으로 파일이 로딩되며 우측 파일에 좌측 파일과 같은 변수가 있으면 덮어씀

##### npm run dev
* .env -> .env.development -> .env.local -> .env.development.local

##### npm run build
* .env -> .env.production -> .env.local -> .env.production.local

## 배포
### Netlify
* https://netlify.com

* netlify 설정 파일 추가
  - 프로젝트 루트에 netlify.toml 파일 생성(들여쓰기는 반드시 스페이스 2개를 이용)
  - fallback url 추가: 클라이언트의 모든 url 요청에 index.html 응답 하도록 설정
```yaml
[[redirects]]
  from = "/*"
  to = "/index.html"
  status = 200
```

* .env 파일을 .gitignore에 추가했을 경우 환경 변수를 수동으로 등록해야 함
  - 배포 설정에서 등록 가능

## 최적화
### SEO(Search Engine Optimization)
#### 메타 데이터 추가
* index.html에 메타 데이터 추가
* SPA는 하나의 html 페이지를 사용하므로 각 페이지별로 메타 데이터가 따로 적용되지 않고 모든 페이지에 일괄 적용됨
  ```html
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/x-icon" href="/images/favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>멋쟁이 사자처럼 커뮤니티 - 멋사컴</title>

    <!-- 기본 meta 태그 -->
    <meta name="description" content="다양한 주제의 커뮤니티와 활발한 소통을 위한 플랫폼입니다. 관심사에 따라 참여하고, 의견을 나누세요." />
    <meta name="keywords" content="커뮤니티, 소통, 포럼, 관심사, 온라인 모임, 커뮤니티 서비스" />
    <meta name="author" content="Front End Boot Camp" />

    <!-- Open Graph meta 태그 (소셜 미디어용) -->
    <meta property="og:title" content="멋사컴에 오신걸 환영합니다." />
    <meta property="og:description" content="유용한 정보를 나누고 공유하세요." />
    <meta property="og:image" content="/images/febc.png" />
    <meta property="og:url" content="https://board.fesp.shop" />
    <meta property="og:type" content="website" />
    <meta property="og:site_name" content="멋사컴" />
  </head>
  ```

#### 페이지별 다른 `<head>` 적용
* 각 페이지 별로 다른 메타 데이터가 적용 되도록 `<head>` 내부의 태그를 동적으로 수정
* react-helmet-async 이용
* sample 하위 각 페이지의 `<head>` 참고
  - List.jsx 예시
    ```jsx
    ......
    return (
      <>
        <Helmet>
          <title>{ type } - 멋사컴</title>
          <meta property="og:title" content={`${ type } 게시판`} />
          <meta property="og:description" content="유용한 정보를 나누고 공유하세요." />
        </Helmet>

        <main className="min-w-80 p-10">
          ......
        </main>
      </>
    );
    ```

### 사용자 경험(UX) 최적화
#### 레이지 로딩
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch05-router#%EB%A0%88%EC%9D%B4%EC%A7%80-%EB%A1%9C%EB%94%A9-lazy-loading

* routes.jsx
  ```jsx
  import { lazy } from 'react';
  ```

  ```jsx
  const Layout = lazy(() => import('@components/layout'));
  const Detail = lazy(() => import('@pages/board/Detail'));
  const Edit = lazy(() => import('@pages/board/Edit'));
  const List = lazy(() => import('@pages/board/List'));
  const New = lazy(() => import('@pages/board/New'));
  const ErrorPage = lazy(() => import('@pages/ErrorPage'));
  const MainPage = lazy(() => import('@pages/index'));
  const Login = lazy(() => import('@pages/user/Login'));
  const Signup = lazy(() => import('@pages/user/Signup'));
  ```

#### 로딩중 상태 표시
##### Spinner 컴포넌트 제작
* components/Spinner.jsx 작성
  ```jsx
  import { HashLoader, ScaleLoader, SkewLoader } from "react-spinners";

  const Spinner = {
    FullScreen(){
      return (
        <div className="fixed inset-0 flex items-center justify-center bg-white dark:bg-gray-700 dark:text-gray-200">
          <div className="flex flex-col items-center">
            <h3 className="mb-4 text-lg font-semibold">잠시만 기다려주세요.</h3>
            <HashLoader
              color="#f58714"
              size={60}
            />
          </div>
        </div>
      );
    },
    WithHeader(){
      const screenHeight = window.innerHeight; // 현재 브라우저 창의 높이
      const headerHeight = 68;
      const footerHeight = 68;
      const spinnerHeight = screenHeight - headerHeight - footerHeight; // 스피너 영역의 높이 계산

      return (
        <div className="flex items-center justify-center" style={{ height: spinnerHeight }}>
          <div className="text-center">
            <h3 className="mb-4 text-lg font-semibold">잠시만 기다려주세요.</h3>
            <SkewLoader color="#F97316" />
          </div>
        </div>
      );    
    },
    TargetArea(){
      return (
        <div className="flex justify-center">
          <ScaleLoader color="#F97316"/>
        </div>
      );
    }
  };

  export default Spinner;
  ```

##### Suspense 컴포넌트 사용
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch09-ajax#render-as-you-fetch
* App.jsx
  ```jsx
  import { Suspense } from "react";
  ```

  ```jsx
  <Suspense fallback={ <Spinner.FullScreen /> }>
    <RouterProvider router={ router } />
  </Suspense>
  ```

#### alert 대신 toast 사용
* 참고: https://github.com/uzoolove/febc11-react/blob/main/workspace-ins/ch09-ajax/02-nike-axios/src/App.jsx#L71

##### toast 설정
* react-toastify 사용
* App.jsx에 `<ToastContainer>` 추가
  ```jsx
  import { Slide, ToastContainer } from 'react-toastify';
  import 'react-toastify/dist/ReactToastify.css';
  ......
  return (
    <Suspense fallback={ <Spinner.FullScreen /> }>
      <RouterProvider router={ router } />
      <ToastContainer
        position="top-center"
        hideProgressBar={true}
        autoClose={1500}
        closeOnClick={true}
        theme="light"
        transition={ Slide }
      />
    </Suspense>
  );
  ```

##### toast 사용 예시
* Detail.jsx
  ```jsx
  import { Slide, toast } from 'react-toastify';
  ......
  const removeItem = useMutation({
    mutationFn: () => axios.delete(`/posts/${_id}`),
    onSuccess: () => {
      // invalidateQueries는 쿼리가 active 상태일때(쿼리를 실행한 컴포넌트가 마운트 됨) refetch를 하고
      // refetchQueries는 쿼리가 inactive 상태일때도(쿼리를 실행한 컴포넌트가 언마운트 됨) refetch를 함
      queryClient.refetchQueries({ queryKey: ['posts', type] });
      toast.success('삭제되었습니다.', {
        onClose: () => navigate(`/${type}`)
      });
    }
  });
  ```

#### NavLink 사용
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch05-router#navlink
* Header.jsx의 Link를 NavLink로 교체
  ```jsx
  <NavLink className={ ({ isActive }) => isActive ? 'text-amber-500 font-semibold' : '' } to="/info">정보공유</NavLink>
  <NavLink className={ ({ isActive }) => isActive ? 'text-amber-500 font-semibold' : '' } to="/free">자유게시판</NavLink>
  <NavLink className={ ({ isActive }) => isActive ? 'text-amber-500 font-semibold' : '' } to="/qna">질문게시판</NavLink>
  ```

#### 에러 처리
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch05-router#%EC%97%90%EB%9F%AC-%EC%B2%98%EB%A6%AC-%EC%A0%84%EC%9A%A9-%EB%9D%BC%EC%9A%B0%ED%8A%B8
* 참고: https://github.com/uzoolove/febc11-react/tree/main/workspace-ins/ch05-router#userouteerror

* 컴포넌트에서 try~catch로 에러를 따로 처리하지 않을 경우 `<ErrorPage>`를 보여줌

##### ErrorPage.jsx
```jsx
import Footer from "@components/layout/Footer";
import Header from "@components/layout/Header";
import { Helmet } from "react-helmet-async";
import { useRouteError } from "react-router-dom";

export default function ErrorPage() {
  const err = useRouteError();
  console.error(err);
  const message = err.status === 404 ? '존재하지 않는 페이지입니다.' : '예상하지 못한 에러가 발생했습니다.';
  return (
    <>
      <Helmet>
        <title>멋쟁이 사자처럼 커뮤니티 - 멋사컴</title>
        <meta property="og:title" content="에러가 발생했어요." />
        <meta property="og:description" content={ message } />
      </Helmet>
      <div className="flex flex-col min-h-screen dark:bg-gray-700 dark:text-gray-200 transition-color duration-500 ease-in-out">
        <Header />
        <div className="py-20 bg-red-100 border border-red-400 text-red-700 p-4 rounded-lg flex flex-col items-center space-y-2">
          <h2 className="text-xl font-semibold mb-2 text-center">🚧 앗, 무언가 잘못됐네요!</h2>
          <h3 className="text-md font-semibold mb-2 text-center">{ message }</h3>
          <p className="pt-12 text-center">이 오류는 더 나은 서비스를 위한 첫걸음이에요. 조금만 기다려 주세요!</p>
          <button onClick={ () => window.location.reload() }
            className="bg-red-600 text-white py-2 px-4 rounded hover:bg-red-700 focus:outline-none focus:ring-2 focus:ring-red-600">
            ⚙️ 다시 시도
          </button>
        </div>
        <Footer />
      </div>
    </>
  );
}
```

##### 에러 페이지 적용
* routex.jsx
  ```jsx
  const router = createBrowserRouter([
    {
      path: "/",
      errorElement: <ErrorPage />,
      element: <Layout />,
      children: [
        ......
      ]
    },
  ]);
  ```