### 로딩 처리

```js
function App() {
  const [movies, setMovies] = useState([]);
  const [isLoading, setIsLoading] = useState(false);

  async function fetchMoviesHandler() {
    setIsLoading(true);
    const response = await fetch("https://swapi.dev/api/films");
    const data = await response.json();

    const transformedMovies = data.results.map((data) => {
      return {
        id: data.episode_id,
        title: data.title,
        openingText: data.opening_crawl,
        releaseDate: data.release_date,
      };
    });

    setMovies(transformedMovies);
    setIsLoading(false);
  }

  if (movies.length > 0) {
    content = <MoviesList movies={movies} />;
  } else if (isLoading) {
    content = <p>로딩 중...</p>;
  }

  return (
    <React.Fragment>
      <section>
        <button onClick={fetchMoviesHandler}>Fetch Movies</button>
      </section>
      <section>{content}</section>
    </React.Fragment>
  );
}
```

1. 데이터 요청 트리거인 `Fetch Movies`버튼을 누르면
2. `isLoading` 은 즉시 true가 되기 때문에 로딩 중... 이라는 문장이 나오게 되고,
3. 데이터 요청 비동기 처리를 끝내게 되면 다시 false로 상태가 바뀌면서 로딩중이라는 문장이 사라지고
4. 영화의 개수가 1개 이상이면 영화 리스트를, 0개이면 영화를 찾지 못했습니다 라는 문장이 나오게 된다.
5. 버튼을 누르기 전 상태도 isLoading이 false이고 받아온 영화 리스트가 없으니 영화를 찾지 못했습니다 라는 문장이 출력된다.

<br/>

### 오류 처리

```js
function App() {
  const [movies, setMovies] = useState([]);
  const [isLoading, setIsLoading] = useState(false);
  const [error, setError] = useState(null);

  async function fetchMoviesHandler() {
    setIsLoading(true);
    setError(null);
    const response = await fetch("https://swapi.dev/api/wrong");
    // response가 제대로 받아와졌는지부터 확인하고 정보를 띄워야됨
    try {
      // ok <- 응답코드가 200번대이면 true 아니면 false
      if (!response.ok) {
        // 만약 오류 상태라면(200번대가 아니라면)
        throw new Error("뭔가 잘못 됐음");
      }
      const data = await response.json();

      const transformedMovies = data.results.map((data) => {
        return {
          id: data.episode_id,
          title: data.title,
          openingText: data.opening_crawl,
          releaseDate: data.release_date,
        };
      });

      setMovies(transformedMovies);
    } catch (error) {
      setError(error.message); // error 객체는 디폴트로 message 프로퍼티를 가지고 있다!!
    }
    setIsLoading(false);
    // 로딩메세지는 성공이든 에러이든 처리가 끝나면 없어져야 됨
  }

  let content = <p>영화를 찾지 못했습니다.</p>;

  if (movies.length > 0) {
    content = <MoviesList movies={movies} />;
  } else if (error) {
    content = <p>{error}</p>;
  } else if (isLoading) {
    content = <p>로딩 중...</p>;
  }

  return (
    <React.Fragment>
      <section>
        <button onClick={fetchMoviesHandler}>Fetch Movies</button>
      </section>
      <section>{content}</section>
    </React.Fragment>
  );
}
```
