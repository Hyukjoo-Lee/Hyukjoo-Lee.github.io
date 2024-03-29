---
title: "{ ReactJS } Basic React Projects_2"
date: 2022-06-14 15:20:00 +07:00
tags: [JavaScript, ReactJS, Mini-project, API, Publishing]
description: "Movie service app using concepts I have learned til now."
---

**Third mini project using concepts I learned & How to publish React App**

### 3. Movie App

#### What I learned
1. When we fetch the API, developers generally use 'async-await' more than 'then'.
```javascript
    const getAPI = async () => {

        const json = await (

            await fetch(
                `url`
            )

        ).json();
        
        setData(json.data.dataset);

    }
```
2. How to exchange props between components.

1. Send props to
```javascript
<Movie key={movie.id} coverImg={movie.medium_cover_image} ... />
```
2. Receive props from
```javascript
    function Movie({ coverImg, ... }) {
    return (
        <div>
            <img src={coverImg} ... />
        </div>
    );
}
```

3. How to set paths; components & routes directory.

### Source Code 1
```javascript
// ../routes/Home.js
import { useEffect, useState } from "react";
import Movie from "../components/Movie";

function Home() {
    const [loading, setLoading] = useState(true)
    const [movies, setMovies] = useState([]);

    // then 보다는 async-await 를 더 보편적으로 사용한다.
    // await 이 await 을 감싸는 구조 (api 불러올 때, 보편적인 방법)
    const getMovies = async () => {

        const json = await (
            await fetch(
                `https://yts.mx/api/v2/list_movies.json?minimum_rating=8.9&sort_by=year`
            )
        ).json();

        setMovies(json.data.movies);
        setLoading(false);
    }

    useEffect(() => {
        getMovies();
    }, []);

    // console.log(movies);
    return (
        <div>
            {loading ? (
                <h1>Loading...</h1>) :
                (<div>
                    {movies.map(movie =>
                        <Movie
                            key={movie.id}
                            coverImg={movie.medium_cover_image}
                            title={movie.title}
                            summary={movie.summary}
                            genres={movie.genres}
                        />)}
                </div>)
            }
        </div>)
}

export default Home;

```

### Source Code 2
```javascript
// ../components/Movie.js
import PropTypes from "prop-types";

// Receive props from another component.
function Movie({ coverImg, title, summary, genres }) {
    return (
        <div>
            <img src={coverImg} alt={title} />
            <h2>{title}</h2>
            <p>{summary}</p>
            <ul>
                <li>
                    {genres.map(g =>
                        <li key={g}>{g}</li>)}
                </li>
            </ul>
        </div>
    );
}

// PropsTypes
Movie.propTypes = {
    coverImg: PropTypes.string.isRequired,
    title: PropTypes.string.isRequired,
    summary: PropTypes.string.isRequired,
    genres: PropTypes.arrayOf(PropTypes.string).isRequired,
}

export default Movie;
```

### Publishing

- npm i gh-pages
- npm run build
- push;
    - Inside package.json, 
     1. add at the end ' "homepage": "https://Hyukjoo-Lee.github.io/ReactJS_movie_web_service" '
     2. "deploy": "gh-pages -d build" ' 
         "predeploy" : "npm run build" (inside script)
     3. Command: npm run deploy
     