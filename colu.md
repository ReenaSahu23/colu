```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Musafir.com - Travel Application</title>
    <link rel="stylesheet" href="styles.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
</head>
<body>
    <header>
        <div class="logo">Musafir.com</div>
        <nav>
            <ul id="nav-bar">
                <li><a href="#">Home</a></li>
                <li><a href="#">Explore</a></li>
                <li><a href="#">About</a></li>
            </ul>
        </nav>
    </header>

    <section class="hero">
        <img src="https://images.unsplash.com/photo-1507525428034-b723cf961d3e?ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D&auto=format&fit=crop&w=2073&q=80" alt="Travel Image">
        <div class="search-bar">
            <input type="text" placeholder="Search your favorite destination...">
            <button>Search</button>
        </div>
    </section>

    <section class="places">
        </section>

    <section class="liked-places">
        <h2 class="liked">Liked Places</h2>
        <div class="liked-places-container">
            </div>
    </section>

    <div id="modal" class="modal">
        <div class="modal-content">
            <span class="close-btn">&times;</span>
            <img id="modal-image" src="" alt="">
            <h3 id="modal-name"></h3>
            <p id="modal-description"></p>
        </div>
    </div>
    
    <footer>
        <div class="footer-section">
            <h3>About Us</h3>
            <p>We are a travel discovery platform dedicated to helping you find the best destinations and experiences. Explore new locations, immerse yourself in cultures, and embark on unforgettable journeys.</p>
        </div>
        <div class="footer-section">
            <h3>Categories</h3>
            <ul>
                <li><a href="#" id="latestDestinationsFooter">Latest</a></li>
                <li><a href="#" id="beachDestinationsFooter">Beach Getaways</a></li>
                <li><a href="#" id="adventureDestinationsFooter">Adventure Trips</a></li>
            </ul>
        </div>
        <div class="footer-section">
            <h3>Contact</h3>
            <ul>
                <li>Email: <a href="mailto:contact@travelapp.com">contact@travelapp.com</a></li>
                <li>Phone: +123-456-7890</li>
            </ul>
        </div>
        <div class="footer-section">
            <h3>Follow Us</h3>
            <div class="social-media-icons">
                <a href="#" aria-label="Facebook">
                    <i class="fab fa-facebook"></i> 
                </a>
                <a href="#" aria-label="Twitter">
                    <i class="fab fa-twitter"></i> 
                </a>
                <a href="#" aria-label="Instagram">
                    <i class="fab fa-instagram"></i>
                </a>
                <a href="#" aria-label="YouTube">
                    <i class="fab fa-youtube"></i> 
                </a>
            </div>
        </div>
    </footer>
    <script src="script.js"></script>
</body>
</html>
```
```js
document.addEventListener('DOMContentLoaded', () => {
    const placesContainer = document.querySelector('.places');
    const likeButtons = document.querySelectorAll('.like-btn');
    const likedPlacesContainer = document.querySelector('.liked-places-container');
    const modal = document.getElementById('modal');
    const modalImage = document.getElementById('modal-image');
    const modalName = document.getElementById('modal-name');
    const modalDescription = document.getElementById('modal-description');
    const closeBtn = document.querySelector('.close-btn');
    const searchInput = document.querySelector('.search-bar input');


    const travelDestinations = [
        {
            city: "Paris, France",
            cost: "$2000",
            duration: "7 Days",
            hotel: "Hotel Le Meurice",
            description: "Explore the city of love, known for its art, fashion, and culture.",
            activities: ["Eiffel Tower visit", "Louvre Museum", "Seine River Cruise"],
            image: "https://m.media-amazon.com/images/I/71oFVzj8piL._AC_UF894,1000_QL80_.jpg",
        },
        {
            city: "New York, USA",
            cost: "$2500",
            duration: "5 Days",
            hotel: "The Plaza Hotel",
            description: "Experience the hustle and bustle of the Big Apple.",
            activities: ["Statue of Liberty", "Times Square", "Central Park"],
            image: "https://i.natgeofe.com/k/5b396b5e-59e7-43a6-9448-708125549aa1/new-york-statue-of-liberty.jpg",
        },
        {
            city: "Tokyo, Japan",
            cost: "$2200",
            duration: "6 Days",
            hotel: "Park Hyatt Tokyo",
            description: "Discover the blend of tradition and technology in Japan’s capital.",
            activities: ["Tokyo Tower", "Shibuya Crossing", "Meiji Shrine"],
            image: "https://images.unsplash.com/photo-1540959733332-eab4deabeeaf?fm=jpg&q=60&w=3000&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxzZWFyY2h8M3x8dG9reW98ZW58MHx8MHx8fDA%3D",
        },
        {
            city: "Sydney, Australia",
            cost: "$3000",
            duration: "7 Days",
            hotel: "Shangri-La Hotel Sydney",
            description: "Experience the beauty of Sydney’s beaches and vibrant city life.",
            activities: ["Sydney Opera House", "Bondi Beach", "Sydney Harbour Bridge"],
            image: "https://media.tatler.com/photos/6141d37b9ce9874a3e40107d/16:9/w_2560%2Cc_limit/social_crop_sydney_opera_house_gettyimages-869714270.jpg",
        },
        {
            city: "Dubai, UAE",
            cost: "$1800",
            duration: "5 Days",
            hotel: "Burj Al Arab",
            description: "A city of luxury, futuristic architecture, and endless shopping.",
            activities: ["Burj Khalifa", "Desert Safari", "Dubai Mall"],
            image: "https://upload.wikimedia.org/wikipedia/commons/c/cc/Dubai_Skylines_at_night_%28Pexels_3787839%29.jpg",
        },
        // Add more destinations as needed
    ];
    

    function displayDestinations(destinations) {
        placesContainer.innerHTML = ''; // Clear previous results
    
        destinations.forEach(destination => {
            const placeCard = document.createElement('div');
            placeCard.classList.add('place-card');
            placeCard.innerHTML = `
                <img src="${destination.image}" alt="${destination.city}">
                <div class="info">
                    <h3>${destination.city}</h3>
                    <p>Cost: ${destination.cost}</p>
                    <p>Duration: ${destination.duration}</p>
                    <p>Top Hotels: ${destination.hotel}</p>
                    <p class="description">${destination.description}</p>
                    <button class="like-btn"><i class="fas fa-heart"></i></button> 
                </div>
            `;
            // ... rest of the code ...
        });
    }
    

    // Function to load liked places from local storage
    function loadLikedPlaces() {
        const likedPlaces = JSON.parse(localStorage.getItem('likedPlaces')) || [];
        likedPlacesContainer.innerHTML = '';
    
        likedPlaces.forEach((place, index) => {
            const placeCard = document.createElement('div');
            placeCard.classList.add('place-card');
            placeCard.innerHTML = `
                <img src="${place.image}" alt="${place.name}">
                <div class="info">
                    <h3>${place.name}</h3>
                    <p>${place.description}</p>
                    <button class="delete-btn" data-index="${index}"><i class="fas fa-trash-alt"></i></button>
                </div>
            `;
    
            const deleteBtn = placeCard.querySelector('.delete-btn');
            deleteBtn.addEventListener('click', () => {
                deletePlace(index);
            });
    
            likedPlacesContainer.appendChild(placeCard);
        });
    }
    

    // Function to handle "like" button click
    function handleLikeButtonClick(event) {
        const placeCard = event.target.closest('.place-card');
        const placeData = {
            name: placeCard.querySelector('h3').innerText,
            image: placeCard.querySelector('img').src,
            description: placeCard.querySelector('.info p:nth-of-type(1)').innerText // Select the first <p> tag for the description
        };
    
        // Retrieve liked places from local storage
        let likedPlaces = JSON.parse(localStorage.getItem('likedPlaces')) || [];
    
        // Check if the place is already liked
        const isAlreadyLiked = likedPlaces.some(place => place.name === placeData.name);
    
        if (!isAlreadyLiked) {
            likedPlaces.push(placeData);
            localStorage.setItem('likedPlaces', JSON.stringify(likedPlaces));
            loadLikedPlaces(); // Reload the liked places section
        } else {
            alert(`${placeData.name} is already liked!`);
        }
    }
    
    
    // Function to delete a place from local storage
    function deletePlace(index) {
        let likedPlaces = JSON.parse(localStorage.getItem('likedPlaces')) || [];
        likedPlaces.splice(index, 1);
        localStorage.setItem('likedPlaces', JSON.stringify(likedPlaces));
        loadLikedPlaces();
    }
    
    function openModal(imageSrc, placeName, placeDescription) {
        modalImage.src = imageSrc;
        modalName.textContent = placeName;
        modalDescription.textContent = placeDescription;
        modal.style.display = 'block';
    }
    
    function closeModal() {
        modal.style.display = 'none';
    }
    
    // Event listeners for opening the modal
    placesContainer.addEventListener('click', (event) => {
        const clickedCard = event.target.closest('.place-card');
        if (clickedCard) {
            const imageSrc = clickedCard.querySelector('img').src;
            const placeName = clickedCard.querySelector('h3').textContent;
            const placeDescription = clickedCard.querySelector('.description').textContent;
            openModal(imageSrc, placeName, placeDescription);
        }
    });
    
    
    // Event listener for closing the modal
    closeBtn.addEventListener('click', closeModal);
    
    // Close the modal when clicking outside of it
    window.onclick = (event) => {
        if (event.target === modal) {
            closeModal();
        }
    }

    // Add event listeners to "like" buttons
    placesContainer.addEventListener('click', (event) => {
        if (event.target.classList.contains('like-btn')) {
            handleLikeButtonClick(event);
        }
    });
    
    searchInput.addEventListener('keyup', function() {
        const query = searchInput.value.toLowerCase();
        const filteredDestinations = travelDestinations.filter(destination => {
            return destination.city.toLowerCase().includes(query);
        });
        displayDestinations(filteredDestinations); 
    });

    displayDestinations(travelDestinations);
    loadLikedPlaces();
});

```
```css
/* styles.css */

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: Arial, sans-serif;
}

header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    background-color: #333;
    padding: 1rem;
    color: white;
}

header .logo {
    font-size: 1.5rem;
    font-weight: bold;
    color: tomato;
}

header nav ul {
    list-style: none;
    display: flex;
    gap: 1.5rem;
}

header nav ul li a {
    color: tomato;
    text-decoration: none;
    transition: color 0.3s ease, text-decoration 0.3s ease;
}

header nav ul li a:hover {
    color: #fff;
    text-decoration: underline;
}

.hero {
    position: relative;
    text-align: center;
    color: white;
}

.hero img {
    width: 100%;
    height: 400px;
    object-fit: cover;
}

.search-bar {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    display: flex;
    border-radius: 25px;
    padding-bottom: 16px;
}

.search-bar input {
    padding: 0.5rem;
    width: 300px;
    border: 2px solid #ccc;
    border-radius: 25px;
    outline: none;
}

.search-bar button {
    padding: 0.5rem;
    background-color: #ff5722;
    color: white;
    border: none;
    border-radius: 25px;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.search-bar button:hover {
    background-color: #e64a19;
}

.places {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 2rem;
    padding: 2rem;
}

.place-card {
    background-color: #f5f5f5;
    padding: 1rem;
    border-radius: 10px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    transition: transform 0.3s ease;
}

.place-card:hover {
    transform: scale(1.05);
}

.place-card img {
    width: 100%;
    height: 200px;
    object-fit: cover;
    border-radius: 10px;
}

.info {
    margin-top: 1rem;
}

.like-btn {
    background: none;
    border: none;
    padding: 0.5rem;
    font-size: 1.2rem;
    cursor: pointer;
    color: #333;
    transition: color 0.3s ease;
}

.like-btn:hover {
    color: tomato;
}

.like-btn.liked {
    color: tomato;
}

.liked-places {
    padding: 2rem;
}

.liked{
    margin: 2rem;
    padding: 1rem;
    color: tomato;
    border-radius: 10px;
    border: 1px solid #050404;
    
}

.liked-places-container {
    display: flex;
    flex-wrap: wrap;
    gap: 38px;
}

.liked-places-container .place-card {
    width: 200px;
    border: 1px solid #ddd;
    padding: 10px;
    border-radius: 10px;
    text-align: center;
}

footer {
    background-color: #0d0d0d;
    color: #e0e0e0;
    padding: 20px;
    display: flex;
    justify-content: space-between;
    flex-wrap: wrap;
    border-top: 2px solid #333;
}

.footer-section {
    margin: 10px;
    flex: 1 1 200px;
}

.footer-section h3 {
    margin-bottom: 10px;
    color: #ff1744;
}

.footer-section ul {
    list-style: none;
    padding: 0;
}

.footer-section ul li {
    margin: 8px 0;
}

.footer-section a {
    color: #e0e0e0;
    text-decoration: none;
    transition: color 0.3s ease;
}

.footer-section a:hover {
    color: #ff1744;
}

.social-media-icons {
    display: flex;
    gap: 10px;
}

.social-media-icons a {
    display: inline-block; /* Make the <a> elements block-level for padding */
    padding: 8px; /* Add padding for better spacing and click area */
    border-radius: 50%; /* Make the icons circular */
    transition: background-color 0.3s ease; /* Add a smooth transition for background color */
}

.social-media-icons a i {
    color: white; 
    font-size: 24px; /* Adjust the font size as needed */
}

.social-media-icons a:hover {
    background-color: #ff1744;
}


.delete-btn {
    background-color: #ff4d4d;
    color: white;
    border: none;
    padding: 5px 10px;
    margin-top: 10px;
    cursor: pointer;
    border-radius: 5px;
    transition: background-color 0.3s ease;
}

.delete-btn:hover {
    background-color: #ff1a1a;
}

.modal {
    display: none;
    position: fixed;
    z-index: 1;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    overflow: auto;
    background-color: rgba(0, 0, 0, 0.4);
}

.modal-content {
    background-color: #fefefe;
    margin: 15% auto;
    padding: 20px;
    border: 1px solid #888;
    width: 80%;
    text-align: center;
}

.close-btn {
    color: #aaa;
    float: right;
    font-size: 28px;
    font-weight: bold;
}

.close-btn:hover,
.close-btn:focus {
    color: black;
    text-decoration: none;
    cursor: pointer;
}

@media only screen and (max-width: 768px) {
    header {
        flex-direction: column;
        text-align: center;
    }

    header nav ul {
        flex-direction: column;
        gap: 1rem;
    }

    .hero img {
        height: 250px;
    }

    .search-bar input {
        width: 200px;
    }

    .places {
        grid-template-columns: 1fr;
    }
}

@media only screen and (max-width: 480px) {
    .search-bar {
        flex-direction: column;
        align-items: center;
    }

    .search-bar input {
        width: 100%;
        margin-bottom: 10px;
    }

    .search-bar button {
        width: 100%;
    }

    .footer-section {
        text-align: center;
    }

    .modal-content {
        width: 90%;
    }
}

#nav-bar {
    padding: 3rem;
    background-color: #333;
    transition: background-color 0.3s ease;
}

#nav-bar:hover {
    background-color: tomato;
}
```