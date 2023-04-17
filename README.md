# Image Gallery App

This is a JavaScript app that uses the Unsplash API to display a gallery of random photos. It loads a set of photos on page load and allows the user to load more images by scrolling to the bottom of the page.

## Getting Started

To get started, clone this repository and open the `index.html` file in your web browser. You will need an internet connection to load the images from the Unsplash API.

## Usage

The app will load a set of 5 photos on page load. To load more photos, scroll to the bottom of the page. The app will automatically load another set of 30 photos.

## Code Overview

The app uses the following JavaScript functions and variables:

```javascript
const imageContainer = document.getElementById('image-container');
const loader = document.getElementById('loader');

let ready = false;
let imagesLoaded = 0;
let totalImages = 0;
let photosArray = [];
let initialLoad = true;

// Unsplash API
let count = 5;
const apiKey = 'kX0nsTQkhtRylre3_i-7qvJBKr3llZTkkPYtZDB2hDU';
let apiUrl = `https://api.unsplash.com/photos/random/?client_id=${apiKey}&count=${count}`;

function updateAPIURLWithNewCount(picCount){
    apiUrl = `https://api.unsplash.com/photos/random/?client_id=${apiKey}&count=${picCount}`;
}

function imageLoaded() {
    imagesLoaded++;    
    if (imagesLoaded === totalImages) {
        ready = true;
        loader.hidden = true;            
    }
}

function setAttributes(element, attributes) {
    for (const key in attributes) {
        element.setAttribute(key, attributes[key]);
    }
}

function displayPhotos() {
    imagesLoaded = 0;
    totalImages = photosArray.length;    
    photosArray.forEach((photo) => {
        const item = document.createElement('a');
        setAttributes(item, {
            href: photo.links.html,
            target: '_blank',
        });
        const img = document.createElement('img');
        setAttributes(img, {
            src: photo.urls.regular,
            alt: photo.alt_description,
            title: photo.alt_description,
        });
        img.addEventListener('load', imageLoaded);
        item.appendChild(img);
        imageContainer.appendChild(item);
    });
}

async function getPhotos () {
    try {
        const response = await fetch(apiUrl);
        photosArray = await response.json();
        displayPhotos();
        if(initialLoad) {
            updateAPIURLWithNewCount(30)
            isInitialLoad = false
        }
    } catch (error) {
        // Catch Error Here
    }
}

window.addEventListener('scroll', () => {
    if (window.innerHeight + window.scrollY >= document.body.offsetHeight - 1000 && ready) {
        ready = false;
        getPhotos();        
    }
});

getPhotos();
