# all--in-one--delivery-service<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>E-commerce with Images, Camera & Maps</title>
<style>
  body { font-family: Arial, sans-serif; background: #eef6fc; margin: 0; padding: 10px; }
  nav, .category-buttons { display: flex; gap: 10px; margin-bottom: 12px; }
  nav button, .category-buttons button {
    padding: 10px 18px; border: none; border-radius: 6px;
    background: #2196f3; color: white; font-weight: bold; cursor: pointer;
  }
  nav button.active, .category-buttons button.active { background: #0b5ed7; }
  .container { max-width: 800px; margin: auto; background: white; padding: 20px; border-radius: 8px; box-shadow: 0 0 7px #ccc;}
  h2, h3 { margin-top: 0; }
  .images-grid { display: grid; grid-template-columns: repeat(5, 1fr); gap: 10px; margin-bottom: 15px; }
  .images-grid img { width: 100%; border-radius: 5px; height: 100px; object-fit: cover; border: 1px solid #ccc; }
  #cameraStream { display: none; border-radius: 10px; width: 100%; max-height: 300px; margin-top: 15px; }
  button.action-btn { padding: 10px 16px; margin-right: 10px; background: #1976d2; border: none; border-radius: 4px; color: #fff; cursor: pointer; font-weight: bold; }
  label { font-weight: bold; display: block; margin-top: 15px; }
  input, textarea { width: 100%; padding: 8px; margin-top: 6px; border-radius: 4px; border: 1px solid #ccc;}
</style>
</head>
<body>

<div class="container">
  <nav>
    <button class="active" onclick="showMainTab('ecommerce')">E-commerce</button>
    <button onclick="showMainTab('food')">Food</button>
    <button onclick="showMainTab('medicines')">Medicines</button>
    <button onclick="showMainTab('others')">Others</button>
  </nav>

  <div id="ecommerce" class="main-tab active-tab">
    <h2>E-commerce Categories</h2>
    <div class="category-buttons" id="ecommerceCats">
      <button class="active" onclick="showCategoryImages('groceries')">Groceries</button>
      <button onclick="showCategoryImages('vegetables')">Vegetables</button>
      <button onclick="showCategoryImages('fruits')">Fruits</button>
      <button onclick="showCategoryImages('meat')">Meat</button>
      <button onclick="showCategoryImages('milk')">Milk</button>
      <button onclick="showCategoryImages('flowers')">Flowers</button>
      <button onclick="showCategoryImages('packets')">Packets</button>
      <button onclick="showCategoryImages('spices')">Spices</button>
      <button onclick="showCategoryImages('dryfruits')">Dry Fruits</button>
      <button onclick="showCategoryImages('others')">Others</button>
    </div>
    <div class="images-grid" id="imageGallery"></div>
  </div>

  <div id="food" class="main-tab" style="display:none;">
    <h2>Food Section</h2>
    <p>Images and details similar to E-commerce can be added here...</p>
  </div>

  <div id="medicines" class="main-tab" style="display:none;">
    <h2>Medicines Section</h2>
    <p>Medicine related images and order form goes here...</p>
  </div>

  <div id="others" class="main-tab" style="display:none;">
    <h2>Others Section</h2>
    <p>Other category images and order forms here...</p>
  </div>

  <hr>

  <h3>Order Form</h3>
  <form id="orderForm">
    <label for="name">Name:</label>
    <input id="name" type="text" required />

    <label for="phone">Phone Number:</label>
    <input id="phone" type="tel" required />

    <label for="address">Delivery Address:</label>
    <textarea id="address" rows="2" required></textarea>

    <label for="orderItems">Order Items List:</label>
    <textarea id="orderItems" rows="2" placeholder="Enter items you want to order" required></textarea>

    <button type="button" class="action-btn" onclick="sendWhatsAppOrder()">Order via WhatsApp</button>
  </form>

  <hr>

  <button class="action-btn" onclick="openCamera()">Open Camera</button>
  <button class="action-btn" onclick="openGoogleMaps()">Open Google Maps</button>

  <video id="cameraStream" autoplay></video>

</div>

<script>
// Simulated image URLs per category (replace with your actual URLs or folder paths)
const categoryImages = {
  groceries: [
    'https://via.placeholder.com/150?text=Groceries+1',
    'https://via.placeholder.com/150?text=Groceries+2',
    'https://via.placeholder.com/150?text=Groceries+3',
    'https://via.placeholder.com/150?text=Groceries+4',
    'https://via.placeholder.com/150?text=Groceries+5',
    'https://via.placeholder.com/150?text=Groceries+6',
    'https://via.placeholder.com/150?text=Groceries+7',
    'https://via.placeholder.com/150?text=Groceries+8',
    'https://via.placeholder.com/150?text=Groceries+9',
    'https://via.placeholder.com/150?text=Groceries+10'
  ],
  vegetables: [
    'https://via.placeholder.com/150?text=Vegetables+1',
    'https://via.placeholder.com/150?text=Vegetables+2',
    'https://via.placeholder.com/150?text=Vegetables+3',
    'https://via.placeholder.com/150?text=Vegetables+4',
    'https://via.placeholder.com/150?text=Vegetables+5',
    'https://via.placeholder.com/150?text=Vegetables+6',
    'https://via.placeholder.com/150?text=Vegetables+7',
    'https://via.placeholder.com/150?text=Vegetables+8',
    'https://via.placeholder.com/150?text=Vegetables+9',
    'https://via.placeholder.com/150?text=Vegetables+10'
  ],
  fruits: [
    'https://via.placeholder.com/150?text=Fruits+1',
    'https://via.placeholder.com/150?text=Fruits+2',
    'https://via.placeholder.com/150?text=Fruits+3',
    'https://via.placeholder.com/150?text=Fruits+4',
    'https://via.placeholder.com/150?text=Fruits+5',
    'https://via.placeholder.com/150?text=Fruits+6',
    'https://via.placeholder.com/150?text=Fruits+7',
    'https://via.placeholder.com/150?text=Fruits+8',
    'https://via.placeholder.com/150?text=Fruits+9',
    'https://via.placeholder.com/150?text=Fruits+10'
  ],
  meat: [
    'https://via.placeholder.com/150?text=Meat+1',
    'https://via.placeholder.com/150?text=Meat+2',
    'https://via.placeholder.com/150?text=Meat+3',
    'https://via.placeholder.com/150?text=Meat+4',
    'https://via.placeholder.com/150?text=Meat+5',
    'https://via.placeholder.com/150?text=Meat+6',
    'https://via.placeholder.com/150?text=Meat+7',
    'https://via.placeholder.com/150?text=Meat+8',
    'https://via.placeholder.com/150?text=Meat+9',
    'https://via.placeholder.com/150?text=Meat+10'
  ],
  milk: [
    'https://via.placeholder.com/150?text=Milk+1',
    'https://via.placeholder.com/150?text=Milk+2',
    'https://via.placeholder.com/150?text=Milk+3',
    'https://via.placeholder.com/150?text=Milk+4',
    'https://via.placeholder.com/150?text=Milk+5',
    'https://via.placeholder.com/150?text=Milk+6',
    'https://via.placeholder.com/150?text=Milk+7',
    'https://via.placeholder.com/150?text=Milk+8',
    'https://via.placeholder.com/150?text=Milk+9',
    'https://via.placeholder.com/150?text=Milk+10'
  ],
  flowers: [
    'https://via.placeholder.com/150?text=Flowers+1',
    'https://via.placeholder.com/150?text=Flowers+2',
    'https://via.placeholder.com/150?text=Flowers+3',
    'https://via.placeholder.com/150?text=Flowers+4',
    'https://via.placeholder.com/150?text=Flowers+5',
    'https://via.placeholder.com/150?text=Flowers+6',
    'https://via.placeholder.com/150?text=Flowers+7',
    'https://via.placeholder.com/150?text=Flowers+8',
    'https://via.placeholder.com/150?text=Flowers+9',
    'https://via.placeholder.com/150?text=Flowers+10'
  ],
  packets: [
    'https://via.placeholder.com/150?text=Packets+1',
    'https://via.placeholder.com/150?text=Packets+2',
    'https://via.placeholder.com/150?text=Packets+3',
    'https://via.placeholder.com/150?text=Packets+4',
    'https://via.placeholder.com/150?text=Packets+5',
    'https://via.placeholder.com/150?text=Packets+6',
    'https://via.placeholder.com/150?text=Packets+7',
    'https://via.placeholder.com/150?text=Packets+8',
    'https://via.placeholder.com/150?text=Packets+9',
    'https://via.placeholder.com/150?text=Packets+10'
  ],
  spices: [
    'https://via.placeholder.com/150?text=Spices+1',
    'https://via.placeholder.com/150?text=Spices+2',
    'https://via.placeholder.com/150?text=Spices+3',
    'https://via.placeholder.com/150?text=Spices+4',
    'https://via.placeholder.com/150?text=Spices+5',
    'https://via.placeholder.com/150?text=Spices+6',
    'https://via.placeholder.com/150?text=Spices+7',
    'https://via.placeholder.com/150?text=Spices+8',
    'https://via.placeholder.com/150?text=Spices+9',
    'https://via.placeholder.com/150?text=Spices+10'
  ],
  dryfruits: [
    'https://via.placeholder.com/150?text=DryFruits+1',
    'https://via.placeholder.com/150?text=DryFruits+2',
    'https://via.placeholder.com/150?text=DryFruits+3',
    'https://via.placeholder.com/150?text=DryFruits+4',
    'https://via.placeholder.com/150?text=DryFruits+5',
    'https://via.placeholder.com/150?text=DryFruits+6',
    'https://via.placeholder.com/150?text=DryFruits+7',
    'https://via.placeholder.com/150?text=DryFruits+8',
    'https://via.placeholder.com/150?text=DryFruits+9',
    'https://via.placeholder.com/150?text=DryFruits+10'
  ],
  others: [
    'https://via.placeholder.com/150?text=Others+1',
    'https://via.placeholder.com/150?text=Others+2',
    'https://via.placeholder.com/150?text=Others+3',
    'https://via.placeholder.com/150?text=Others+4',
    'https://via.placeholder.com/150?text=Others+5',
    'https://via.placeholder.com/150?text=Others+6',
    'https://via.placeholder.com/150?text=Others+7',
    'https://via.placeholder.com/150?text=Others+8',
    'https://via.placeholder.com/150?text=Others+9',
    'https://via.placeholder.com/150?text=Others+10'
  ],
};

function showMainTab(tab) {
  document.querySelectorAll('.main-tab').forEach(div => div.style.display = 'none');
  document.getElementById(tab).style.display = 'block';
  document.querySelectorAll('nav button').forEach(btn => btn.classList.remove('active'));
  event.target.classList.add('active');
  if (tab === 'ecommerce') {
    showCategoryImages('groceries');
  }
}

function showCategoryImages(category) {
  const gallery = document.getElementById('imageGallery');
  gallery.innerHTML = '';
  const images = categoryImages[category] || [];

  document.querySelectorAll('#ecommerceCats button').forEach(btn => btn.classList.remove('active'));
  event.target.classList.add('active');

  images.forEach(src => {
    const img = document.createElement('img');
    img.src = src;
    img.alt = category + ' image';
    gallery.appendChild(img);
  });
}

function sendWhatsAppOrder() {
  const name = document.getElementById('name').value.trim();
  const phone = document.getElementById('phone').value.trim();
  const address = document.getElementById('address').value.trim();
  const orderItems = document.getElementById('orderItems').value.trim();

  if (!name || !phone || !address || !orderItems) {
    alert('Please fill all fields.');
    return;
  }

  // Remember to change this to your WhatsApp number with country code
  const whatsappNumber = '919876543210'; 

  const message = `Order Details:%0AName: ${encodeURIComponent(name)}%0APhone: ${encodeURIComponent(phone)}%0ADelivery Address: ${encodeURIComponent(address)}%0AOrder Items:%0A${encodeURIComponent(orderItems)}`;
  const url = `https://wa.me/${whatsappNumber}?text=${message}`;
  window.open(url, '_blank');
}

// Camera open using WebRTC getUserMedia API
function openCamera() {
  const video = document.getElementById('cameraStream');
  if (!navigator.mediaDevices || !navigator.mediaDevices.getUserMedia) {
    alert('Camera API not supported in this browser.');
    return;
  }
  navigator.mediaDevices.getUserMedia({ video: true })
    .then(stream => {
      video.srcObject = stream;
      video.style.display = 'block';
    })
    .catch(err => alert('Could not access the camera: ' + err));
}

// Open Google Maps in new tab centered on a fixed location or user's location if available
function openGoogleMaps() {
  // Trying to get user's current position to open Google Maps centered there
  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition(position => {
      const lat = position.coords.latitude;
      const lon = position.coords.longitude;
      const googleMapsUrl = `https://www.google.com/maps/@${lat},${lon},15z`;
      window.open(googleMapsUrl, '_blank');
    }, () => {
      // fallback if user denies location permission
      window.open('https://www.google.com/maps', '_blank');
    });
  } else {
    window.open('https://www.google.com/maps', '_blank');
  }
}

// Initialize default
document.addEventListener('DOMContentLoaded', () => {
  showCategoryImages('groceries');
});
</script>

</body>
</html>
