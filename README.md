<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Zoho Ride - Ride Sharing Application</title>
    <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
    <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
    <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
    <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" />
    <script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js"></script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            padding: 20px;
        }

        .app-container {
            max-width: 1200px;
            margin: 0 auto;
            background: white;
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            overflow: hidden;
            min-height: 90vh;
        }

        /* Logo and Branding Styles */
        .app-logo {
            display: flex;
            align-items: center;
            gap: 12px;
        }

        .logo-icon {
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 8px;
            overflow: hidden;
            box-shadow: 0 4px 8px rgba(0,0,0,0.1);
        }

        .logo-icon img {
            width: 100%;
            height: 100%;
            object-fit: contain;
        }

        .app-name {
            font-size: 1.5rem;
            font-weight: 700;
            color: white;
            letter-spacing: -0.5px;
        }

        /* Notification Styles */
        .notification-container {
            position: fixed;
            top: 20px;
            right: 20px;
            z-index: 1000;
            max-width: 400px;
        }

        .notification {
            background: white;
            padding: 1rem;
            margin-bottom: 0.5rem;
            border-radius: 10px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            border-left: 4px solid #667eea;
            animation: slideIn 0.3s ease-out;
        }

        .notification.success {
            border-left-color: #28a745;
        }

        .notification.error {
            border-left-color: #dc3545;
        }

        .notification.warning {
            border-left-color: #ffc107;
        }

        .notification.info {
            border-left-color: #17a2b8;
        }

        @keyframes slideIn {
            from {
                transform: translateX(100%);
                opacity: 0;
            }
            to {
                transform: translateX(0);
                opacity: 1;
            }
        }

        .notification-header {
            display: flex;
            justify-content: between;
            align-items: center;
            margin-bottom: 0.5rem;
        }

        .notification-title {
            font-weight: bold;
            color: #333;
        }

        .notification-close {
            background: none;
            border: none;
            font-size: 1.2rem;
            cursor: pointer;
            color: #666;
            width: auto;
            padding: 0;
        }

        .notification-message {
            color: #666;
            font-size: 0.9rem;
        }

        /* Rest of the styles remain the same */
        .auth-container {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 80vh;
            padding: 20px;
        }

        .auth-form {
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            width: 100%;
            max-width: 400px;
        }

        .auth-form h2 {
            text-align: center;
            margin-bottom: 1.5rem;
            color: #333;
            font-size: 1.8rem;
        }

        .form-group {
            margin-bottom: 1rem;
        }

        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: #555;
        }

        .form-group input,
        .form-group select {
            width: 100%;
            padding: 12px;
            border: 2px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
            transition: border-color 0.3s;
        }

        .form-group input:focus,
        .form-group select:focus {
            outline: none;
            border-color: #667eea;
        }

        button {
            padding: 12px;
            background: linear-gradient(135deg, #FF6B35 0%, #F7931E 100%);
            color: white;
            border: none;
            border-radius: 8px;
            font-size: 1rem;
            font-weight: 600;
            cursor: pointer;
            transition: transform 0.2s;
        }

        button:hover {
            transform: translateY(-2px);
        }

        button:disabled {
            background: #ccc;
            transform: none;
            cursor: not-allowed;
        }

        .error-message {
            background: #fee;
            color: #c33;
            padding: 0.75rem;
            border-radius: 8px;
            margin-bottom: 1rem;
            border: 1px solid #fcc;
            text-align: center;
        }

        .success-message {
            background: #efe;
            color: #363;
            padding: 0.75rem;
            border-radius: 8px;
            margin-bottom: 1rem;
            border: 1px solid #cfc;
            text-align: center;
        }

        .link-text {
            text-align: center;
            margin-top: 1rem;
            color: #666;
        }

        .link-text a {
            color: #FF6B35;
            text-decoration: none;
            font-weight: 600;
        }

        .link-text a:hover {
            text-decoration: underline;
        }

        .dashboard {
            min-height: 80vh;
        }

        .dashboard-header {
            background: linear-gradient(135deg, #FF6B35 0%, #F7931E 100%);
            color: white;
            padding: 1rem 2rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            box-shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

        .dashboard-header h1 {
            font-size: 1.5rem;
        }

        .user-info {
            display: flex;
            align-items: center;
            gap: 1rem;
        }

        .logout-btn {
            background: rgba(255,255,255,0.2);
            color: white;
            border: 1px solid rgba(255,255,255,0.3);
            width: auto;
            padding: 8px 16px;
        }

        .logout-btn:hover {
            background: rgba(255,255,255,0.3);
        }

        .dashboard-content {
            padding: 2rem;
            max-width: 1200px;
            margin: 0 auto;
        }

        .map-container {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 2rem;
            margin-bottom: 2rem;
        }

        @media (max-width: 768px) {
            .map-container {
                grid-template-columns: 1fr;
            }
        }

        .map-section {
            background: white;
            padding: 1.5rem;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }

        .map-section h3 {
            margin-bottom: 1rem;
            color: #333;
            text-align: center;
        }

        #pickupMap, #destinationMap {
            height: 300px;
            border-radius: 10px;
            margin-bottom: 1rem;
        }

        .location-inputs {
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .location-display {
            background: #f8f9fa;
            padding: 1rem;
            border-radius: 8px;
            margin-top: 1rem;
        }

        .location-display p {
            margin: 0.25rem 0;
            font-size: 0.9rem;
        }

        .map-controls {
            display: flex;
            gap: 1rem;
            margin-top: 1rem;
        }

        .map-controls button {
            flex: 1;
        }

        .use-current-location {
            background: linear-gradient(135deg, #28a745 0%, #20c997 100%);
        }

        .confirm-location {
            background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);
        }

        .ride-booking {
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            margin-bottom: 2rem;
        }

        .ride-booking h2 {
            margin-bottom: 1.5rem;
            color: #333;
            text-align: center;
        }

        .request-btn {
            background: linear-gradient(135deg, #28a745 0%, #20c997 100%);
            font-size: 1.1rem;
            padding: 15px;
            width: 100%;
        }

        .cancel-btn {
            background: linear-gradient(135deg, #dc3545 0%, #e83e8c 100%);
            width: 100%;
        }

        .ride-status {
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            text-align: center;
        }

        .status-badge {
            display: inline-block;
            padding: 8px 16px;
            background: #FF6B35;
            color: white;
            border-radius: 20px;
            font-weight: 600;
            margin: 1rem 0;
        }

        .ride-info {
            text-align: left;
            background: #f8f9fa;
            padding: 1.5rem;
            border-radius: 10px;
            margin: 1rem 0;
        }

        .ride-info p {
            margin-bottom: 0.5rem;
            font-size: 1.1rem;
        }

        .availability-toggle {
            background: white;
            padding: 1.5rem;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            margin-bottom: 2rem;
            display: flex;
            align-items: center;
            gap: 1rem;
            justify-content: center;
        }

        .toggle-btn {
            width: auto;
            padding: 10px 20px;
            font-size: 1rem;
        }

        .toggle-btn.available {
            background: linear-gradient(135deg, #28a745 0%, #20c997 100%);
        }

        .toggle-btn.unavailable {
            background: linear-gradient(135deg, #6c757d 0%, #495057 100%);
        }

        .ride-requests {
            background: white;
            padding: 1.5rem;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }

        .ride-request-card {
            border: 2px solid #e9ecef;
            padding: 1.5rem;
            margin-bottom: 1rem;
            border-radius: 10px;
            transition: all 0.3s;
        }

        .ride-request-card:hover {
            border-color: #FF6B35;
            transform: translateY(-2px);
        }

        .ride-request-card h4 {
            margin-bottom: 1rem;
            color: #333;
        }

        .accept-btn {
            background: linear-gradient(135deg, #007bff 0%, #0056b3 100%);
            width: auto;
            padding: 10px 20px;
        }

        .current-ride {
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            margin-bottom: 2rem;
        }

        .ride-actions {
            display: flex;
            gap: 1rem;
            margin-top: 1.5rem;
            justify-content: center;
        }

        .ride-actions button {
            width: auto;
            padding: 10px 20px;
            flex: 1;
        }

        .map-marker-popup {
            text-align: center;
        }

        .map-marker-popup button {
            margin-top: 0.5rem;
            padding: 8px 12px;
            font-size: 0.9rem;
        }

        .new-ride-notification {
            background: linear-gradient(135deg, #ff6b6b 0%, #ee5a24 100%);
            color: white;
            padding: 1rem;
            border-radius: 10px;
            margin-bottom: 1rem;
            animation: pulse 2s infinite;
        }

        /* New Styles for Ride History */
        .history-section {
            background: white;
            padding: 2rem;
            border-radius: 15px;
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
            margin-top: 2rem;
        }

        .history-section h2 {
            margin-bottom: 1.5rem;
            color: #333;
            text-align: center;
        }

        .history-tabs {
            display: flex;
            gap: 1rem;
            margin-bottom: 1.5rem;
            border-bottom: 2px solid #e9ecef;
        }

        .history-tab {
            padding: 0.75rem 1.5rem;
            background: none;
            border: none;
            border-bottom: 3px solid transparent;
            cursor: pointer;
            font-weight: 600;
            color: #666;
            transition: all 0.3s;
        }

        .history-tab.active {
            color: #FF6B35;
            border-bottom-color: #FF6B35;
        }

        .history-list {
            display: flex;
            flex-direction: column;
            gap: 1rem;
        }

        .history-card {
            border: 2px solid #e9ecef;
            padding: 1.5rem;
            border-radius: 10px;
            transition: all 0.3s;
        }

        .history-card:hover {
            border-color: #FF6B35;
            transform: translateY(-2px);
        }

        .history-card.completed {
            border-left: 4px solid #28a745;
        }

        .history-card.cancelled {
            border-left: 4px solid #dc3545;
        }

        .history-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 1rem;
        }

        .history-date {
            color: #666;
            font-size: 0.9rem;
        }

        .history-status {
            padding: 4px 12px;
            border-radius: 15px;
            font-size: 0.8rem;
            font-weight: 600;
        }

        .status-completed {
            background: #d4edda;
            color: #155724;
        }

        .status-cancelled {
            background: #f8d7da;
            color: #721c24;
        }

        .history-details {
            display: grid;
            grid-template-columns: 1fr auto 1fr;
            gap: 1rem;
            align-items: center;
        }

        .location-from, .location-to {
            text-align: center;
        }

        .location-icon {
            font-size: 1.5rem;
            margin-bottom: 0.5rem;
        }

        .route-arrow {
            font-size: 2rem;
            color: #FF6B35;
        }

        .history-footer {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 1rem;
            padding-top: 1rem;
            border-top: 1px solid #e9ecef;
        }

        .history-fare {
            font-size: 1.2rem;
            font-weight: bold;
            color: #28a745;
        }

        .history-distance {
            color: #666;
        }

        .no-history {
            text-align: center;
            padding: 2rem;
            color: #666;
        }

        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
    </style>
</head>
<body>
    <div id="root"></div>
    <div id="notification-container"></div>

    <script type="text/babel">
        const { useState, useEffect, createContext, useContext } = React;

        // Global Notification System
        class NotificationSystem {
            static showNotification(title, message, type = 'info', duration = 5000) {
                const container = document.getElementById('notification-container');
                if (!container) return;

                const notification = document.createElement('div');
                notification.className = `notification ${type}`;
                notification.innerHTML = `
                    <div class="notification-header">
                        <div class="notification-title">${title}</div>
                        <button class="notification-close" onclick="this.parentElement.parentElement.remove()">×</button>
                    </div>
                    <div class="notification-message">${message}</div>
                `;

                container.appendChild(notification);

                if (duration > 0) {
                    setTimeout(() => {
                        if (notification.parentElement) {
                            notification.remove();
                        }
                    }, duration);
                }
            }
        }

        // Global Event Bus for real-time communication
        class EventBus {
            constructor() {
                this.events = {};
            }

            on(event, callback) {
                if (!this.events[event]) {
                    this.events[event] = [];
                }
                this.events[event].push(callback);
            }

            emit(event, data) {
                if (this.events[event]) {
                    this.events[event].forEach(callback => callback(data));
                }
            }

            off(event, callback) {
                if (this.events[event]) {
                    this.events[event] = this.events[event].filter(cb => cb !== callback);
                }
            }
        }

        // Create global event bus
        const eventBus = new EventBus();

        // Map Service
        class MapService {
            constructor() {
                this.defaultLocation = [12.9716, 77.5946];
            }

            async geocodeAddress(address) {
                await new Promise(resolve => setTimeout(resolve, 1000));
                
                const mockLocations = {
                    'mg road': [12.9758, 77.6047],
                    'indiranagar': [12.9784, 77.6408],
                    'koramangala': [12.9279, 77.6271],
                    'whitefield': [12.9698, 77.7499],
                    'electronic city': [12.8459, 77.6604],
                    'hebbal': [13.0355, 77.5970],
                    'yeshwanthpur': [13.0255, 77.5460],
                    'jayanagar': [12.9308, 77.5838],
                    'marathahalli': [12.9592, 77.6974],
                    'btm layout': [12.9166, 77.6101]
                };

                const normalizedAddress = address.toLowerCase();
                for (const [key, coords] of Object.entries(mockLocations)) {
                    if (normalizedAddress.includes(key)) {
                        return {
                            lat: coords[0],
                            lng: coords[1],
                            address: address,
                            displayName: key.charAt(0).toUpperCase() + key.slice(1)
                        };
                    }
                }

                return {
                    lat: this.defaultLocation[0] + (Math.random() - 0.5) * 0.1,
                    lng: this.defaultLocation[1] + (Math.random() - 0.5) * 0.1,
                    address: address,
                    displayName: address
                };
            }

            async reverseGeocode(lat, lng) {
                await new Promise(resolve => setTimeout(resolve, 800));
                
                const areas = [
                    "MG Road", "Indiranagar", "Koramangala", "Whitefield", "Electronic City",
                    "Hebbal", "Yeshwanthpur", "Jayanagar", "Marathahalli", "BTM Layout",
                    "HSR Layout", "Bellandur", "Sarjapur Road", "Bannerghatta Road"
                ];
                
                const randomArea = areas[Math.floor(Math.random() * areas.length)];
                return {
                    address: `${randomArea}, Bangalore`,
                    displayName: randomArea,
                    lat: lat,
                    lng: lng
                };
            }

            calculateDistance(lat1, lon1, lat2, lon2) {
                const R = 6371;
                const dLat = (lat2 - lat1) * Math.PI / 180;
                const dLon = (lon2 - lon1) * Math.PI / 180;
                const a = 
                    Math.sin(dLat/2) * Math.sin(dLat/2) +
                    Math.cos(lat1 * Math.PI / 180) * Math.cos(lat2 * Math.PI / 180) * 
                    Math.sin(dLon/2) * Math.sin(dLon/2);
                const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
                return R * c;
            }

            calculateFare(distanceKm, vehicleType = 'car') {
                const baseFare = {
                    'bike': 30,
                    'auto': 40,
                    'car': 50
                }[vehicleType] || 50;

                const perKmRate = {
                    'bike': 10,
                    'auto': 15,
                    'car': 18
                }[vehicleType] || 18;

                return Math.round(baseFare + (distanceKm * perKmRate));
            }
        }

        // Enhanced Mock Backend with Ride History
        class MockBackend {
            constructor() {
                this.users = JSON.parse(localStorage.getItem('users')) || [];
                this.rides = JSON.parse(localStorage.getItem('rides')) || [];
                this.currentUser = JSON.parse(localStorage.getItem('currentUser')) || null;
                this.mapService = new MapService();
                
                if (this.users.length === 0) {
                    this.initializeDemoUsers();
                }

                // Listen for ride requests
                eventBus.on('newRideRequest', (ride) => {
                    this.broadcastToDrivers(ride);
                });

                eventBus.on('rideAccepted', (data) => {
                    this.notifyRider(data.rideId, data.driver);
                });
            }

            initializeDemoUsers() {
                const demoUsers = [
                    {
                        id: '1',
                        name: 'John Rider',
                        email: 'rider@example.com',
                        password: 'password123',
                        phone: '+91 9876543210',
                        userType: 'rider',
                        createdAt: new Date().toISOString()
                    },
                    {
                        id: '2',
                        name: 'Mike Driver',
                        email: 'driver@example.com',
                        password: 'password123',
                        phone: '+91 9876543211',
                        userType: 'driver',
                        vehicleType: 'Car',
                        vehicleNumber: 'KA01AB1234',
                        isAvailable: false,
                        createdAt: new Date().toISOString()
                    }
                ];
                
                this.users = demoUsers;
                localStorage.setItem('users', JSON.stringify(this.users));
            }

            delay(ms) {
                return new Promise(resolve => setTimeout(resolve, ms));
            }

            async register(userData) {
                await this.delay(1000);
                
                const existingUser = this.users.find(u => u.email === userData.email);
                if (existingUser) {
                    throw new Error('User already exists');
                }

                const user = {
                    id: Date.now().toString(),
                    ...userData,
                    createdAt: new Date().toISOString()
                };

                this.users.push(user);
                localStorage.setItem('users', JSON.stringify(this.users));
                
                this.currentUser = user;
                localStorage.setItem('currentUser', JSON.stringify(user));

                return { user, token: 'mock-jwt-token' };
            }

            async login(email, password) {
                await this.delay(1000);
                
                const user = this.users.find(u => u.email === email && u.password === password);
                if (!user) {
                    throw new Error('Invalid email or password');
                }

                this.currentUser = user;
                localStorage.setItem('currentUser', JSON.stringify(user));

                return { user, token: 'mock-jwt-token' };
            }

            async requestRide(pickupLocation, destination) {
                await this.delay(1500);
                
                const distance = this.mapService.calculateDistance(
                    pickupLocation.lat, pickupLocation.lng,
                    destination.lat, destination.lng
                );
                
                const fare = this.mapService.calculateFare(distance);

                const ride = {
                    id: Date.now().toString(),
                    riderId: this.currentUser.id,
                    riderName: this.currentUser.name,
                    riderPhone: this.currentUser.phone,
                    pickupLocation,
                    destination,
                    status: 'requested',
                    fare: fare,
                    distance: parseFloat(distance.toFixed(2)),
                    duration: Math.round(distance * 3 + 5),
                    createdAt: new Date().toISOString(),
                    completedAt: null
                };

                this.rides.push(ride);
                localStorage.setItem('rides', JSON.stringify(this.rides));

                // Broadcast to all drivers via event bus
                eventBus.emit('newRideRequest', ride);

                // Show notification to rider
                NotificationSystem.showNotification(
                    'Ride Requested!', 
                    'Finding nearby drivers...', 
                    'success'
                );

                return ride;
            }

            broadcastToDrivers(ride) {
                console.log('Broadcasting ride to drivers:', ride);
                
                // Show notification to all driver components
                eventBus.emit('driverNotification', {
                    type: 'new_ride',
                    ride: ride,
                    message: `New ride request from ${ride.pickupLocation.address} to ${ride.destination.address} - ₹${ride.fare}`
                });

                // Auto-accept by demo driver after 3 seconds if available
                setTimeout(() => {
                    const demoDriver = this.users.find(u => u.userType === 'driver' && u.isAvailable);
                    console.log('Auto-accept check - Demo driver:', demoDriver);
                    if (demoDriver && this.getRideById(ride.id)?.status === 'requested') {
                        console.log('Auto-accepting ride');
                        this.acceptRide(ride.id, demoDriver.id);
                    }
                }, 3000);
            }

            getRideById(rideId) {
                return this.rides.find(r => r.id === rideId);
            }

            async acceptRide(rideId, driverId = this.currentUser.id) {
                await this.delay(1000);
                
                const ride = this.rides.find(r => r.id === rideId);
                if (ride) {
                    const driver = this.users.find(u => u.id === driverId);
                    
                    ride.status = 'accepted';
                    ride.driverId = driver.id;
                    ride.driverName = driver.name;
                    ride.driverPhone = driver.phone;
                    ride.vehicleType = driver.vehicleType;
                    ride.vehicleNumber = driver.vehicleNumber;
                    
                    localStorage.setItem('rides', JSON.stringify(this.rides));

                    // Notify rider
                    eventBus.emit('rideAccepted', {
                        rideId: ride.id,
                        driver: driver
                    });

                    // Show notification to driver
                    NotificationSystem.showNotification(
                        'Ride Accepted!', 
                        `You are now driving to ${ride.riderName}`,
                        'success'
                    );

                    return ride;
                }
                throw new Error('Ride not found');
            }

            notifyRider(rideId, driver) {
                eventBus.emit('riderNotification', {
                    type: 'ride_accepted',
                    rideId: rideId,
                    driver: driver,
                    message: `Your ride has been accepted by ${driver.name} (${driver.vehicleType} - ${driver.vehicleNumber})`
                });
            }

            getCurrentRide() {
                return this.rides.find(ride => 
                    (ride.riderId === this.currentUser.id || ride.driverId === this.currentUser.id) &&
                    ['requested', 'accepted', 'arrived', 'ongoing'].includes(ride.status)
                );
            }

            async updateRideStatus(rideId, status) {
                await this.delay(500);
                
                const ride = this.rides.find(r => r.id === rideId);
                if (ride) {
                    ride.status = status;
                    
                    // Set completedAt timestamp when ride is completed
                    if (status === 'completed') {
                        ride.completedAt = new Date().toISOString();
                    }
                    
                    localStorage.setItem('rides', JSON.stringify(this.rides));

                    // Notify both rider and driver about status change
                    eventBus.emit('rideStatusUpdate', {
                        rideId: rideId,
                        status: status,
                        ride: ride
                    });

                    return ride;
                }
                throw new Error('Ride not found');
            }

            getRideRequests() {
                return this.rides.filter(ride => ride.status === 'requested');
            }

            // Get ride history for current user
            getRideHistory() {
                return this.rides
                    .filter(ride => 
                        (ride.riderId === this.currentUser.id || ride.driverId === this.currentUser.id) &&
                        ['completed', 'cancelled'].includes(ride.status)
                    )
                    .sort((a, b) => new Date(b.completedAt || b.createdAt) - new Date(a.completedAt || a.createdAt));
            }

            // Get completed rides for current user
            getCompletedRides() {
                return this.rides
                    .filter(ride => 
                        (ride.riderId === this.currentUser.id || ride.driverId === this.currentUser.id) &&
                        ride.status === 'completed'
                    )
                    .sort((a, b) => new Date(b.completedAt) - new Date(a.completedAt));
            }

            // Get cancelled rides for current user
            getCancelledRides() {
                return this.rides
                    .filter(ride => 
                        (ride.riderId === this.currentUser.id || ride.driverId === this.currentUser.id) &&
                        ride.status === 'cancelled'
                    )
                    .sort((a, b) => new Date(b.createdAt) - new Date(a.createdAt));
            }

            updateDriverAvailability(driverId, isAvailable) {
                const driver = this.users.find(u => u.id === driverId);
                if (driver) {
                    driver.isAvailable = isAvailable;
                    localStorage.setItem('users', JSON.stringify(this.users));
                    console.log(`Driver ${driver.name} availability set to: ${isAvailable}`);
                }
            }

            logout() {
                this.currentUser = null;
                localStorage.removeItem('currentUser');
            }
        }

        // Create mock backend instance
        const mockBackend = new MockBackend();

        // Auth Context
        const AuthContext = createContext();

        const AuthProvider = ({ children }) => {
            const [user, setUser] = useState(mockBackend.currentUser);
            const [loading, setLoading] = useState(false);

            const register = async (userData) => {
                setLoading(true);
                try {
                    const { user } = await mockBackend.register(userData);
                    setUser(user);
                    return { success: true };
                } catch (error) {
                    return { success: false, message: error.message };
                } finally {
                    setLoading(false);
                }
            };

            const login = async (email, password) => {
                setLoading(true);
                try {
                    const { user } = await mockBackend.login(email, password);
                    setUser(user);
                    return { success: true };
                } catch (error) {
                    return { success: false, message: error.message };
                } finally {
                    setLoading(false);
                }
            };

            const logout = () => {
                mockBackend.logout();
                setUser(null);
            };

            return (
                <AuthContext.Provider value={{ user, register, login, logout, loading }}>
                    {children}
                </AuthContext.Provider>
            );
        };

        const useAuth = () => useContext(AuthContext);

        // Logo Component with Image
        const AppLogo = ({ size = 'medium' }) => {
            const sizes = {
                small: { width: '30px', height: '30px' },
                medium: { width: '40px', height: '40px' },
                large: { width: '50px', height: '50px' }
            };
            
            const style = sizes[size] || sizes.medium;
            
            return (
                <div className="app-logo">
                    <div className="logo-icon" style={style}>
                        <img 
                            src="Gemini_Generated_Image_4x08i84x08i84x08.png" 
                            alt="Zoho Ride" 
                        />
                    </div>
                    <div className="app-name">Zoho Ride</div>
                </div>
            );
        };

        // Ride History Component
        const RideHistory = ({ userType }) => {
            const [activeTab, setActiveTab] = useState('all');
            const [rideHistory, setRideHistory] = useState([]);

            useEffect(() => {
                loadRideHistory();
            }, [activeTab]);

            const loadRideHistory = () => {
                let history = [];
                switch (activeTab) {
                    case 'completed':
                        history = mockBackend.getCompletedRides();
                        break;
                    case 'cancelled':
                        history = mockBackend.getCancelledRides();
                        break;
                    default:
                        history = mockBackend.getRideHistory();
                        break;
                }
                setRideHistory(history);
            };

            const formatDate = (dateString) => {
                const date = new Date(dateString);
                return date.toLocaleDateString('en-IN', {
                    day: 'numeric',
                    month: 'short',
                    year: 'numeric',
                    hour: '2-digit',
                    minute: '2-digit'
                });
            };

            const getStatusClass = (status) => {
                return status === 'completed' ? 'status-completed' : 'status-cancelled';
            };

            return (
                <div className="history-section">
                    <h2>📋 Ride History</h2>
                    
                    <div className="history-tabs">
                        <button 
                            className={`history-tab ${activeTab === 'all' ? 'active' : ''}`}
                            onClick={() => setActiveTab('all')}
                        >
                            All Rides
                        </button>
                        <button 
                            className={`history-tab ${activeTab === 'completed' ? 'active' : ''}`}
                            onClick={() => setActiveTab('completed')}
                        >
                            Completed
                        </button>
                        <button 
                            className={`history-tab ${activeTab === 'cancelled' ? 'active' : ''}`}
                            onClick={() => setActiveTab('cancelled')}
                        >
                            Cancelled
                        </button>
                    </div>

                    <div className="history-list">
                        {rideHistory.length === 0 ? (
                            <div className="no-history">
                                <p>No {activeTab !== 'all' ? activeTab : ''} rides found</p>
                                <p style={{ marginTop: '0.5rem', color: '#999' }}>
                                    {userType === 'rider' 
                                        ? 'Book your first ride to see history here!' 
                                        : 'Complete your first ride to see history here!'}
                                </p>
                            </div>
                        ) : (
                            rideHistory.map(ride => (
                                <div key={ride.id} className={`history-card ${ride.status}`}>
                                    <div className="history-header">
                                        <div className="history-date">
                                            {formatDate(ride.completedAt || ride.createdAt)}
                                        </div>
                                        <div className={`history-status ${getStatusClass(ride.status)}`}>
                                            {ride.status.toUpperCase()}
                                        </div>
                                    </div>
                                    
                                    <div className="history-details">
                                        <div className="location-from">
                                            <div className="location-icon">📍</div>
                                            <div>
                                                <strong>From</strong>
                                                <p>{ride.pickupLocation.address}</p>
                                            </div>
                                        </div>
                                        
                                        <div className="route-arrow">➡️</div>
                                        
                                        <div className="location-to">
                                            <div className="location-icon">🎯</div>
                                            <div>
                                                <strong>To</strong>
                                                <p>{ride.destination.address}</p>
                                            </div>
                                        </div>
                                    </div>
                                    
                                    <div className="history-footer">
                                        <div className="history-fare">₹{ride.fare}</div>
                                        <div className="history-distance">
                                            {ride.distance} km • {ride.duration} min
                                        </div>
                                    </div>
                                    
                                    {userType === 'rider' && ride.driverName && (
                                        <div style={{ marginTop: '1rem', paddingTop: '1rem', borderTop: '1px solid #e9ecef' }}>
                                            <p><strong>Driver:</strong> {ride.driverName}</p>
                                            <p><strong>Vehicle:</strong> {ride.vehicleType} - {ride.vehicleNumber}</p>
                                        </div>
                                    )}
                                    
                                    {userType === 'driver' && (
                                        <div style={{ marginTop: '1rem', paddingTop: '1rem', borderTop: '1px solid #e9ecef' }}>
                                            <p><strong>Rider:</strong> {ride.riderName}</p>
                                            <p><strong>Phone:</strong> {ride.riderPhone}</p>
                                        </div>
                                    )}
                                </div>
                            ))
                        )}
                    </div>
                </div>
            );
        };

        // Map Component
        const MapSelector = ({ onLocationSelect, type, defaultLocation }) => {
            const [map, setMap] = useState(null);
            const [marker, setMarker] = useState(null);
            const [selectedLocation, setSelectedLocation] = useState(null);

            useEffect(() => {
                let newMap;
                
                if (!map) {
                    newMap = L.map(`${type}Map`).setView(
                        defaultLocation || [12.9716, 77.5946], 
                        13
                    );

                    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
                        attribution: '© OpenStreetMap contributors'
                    }).addTo(newMap);

                    setMap(newMap);

                    const clickHandler = async (e) => {
                        const { lat, lng } = e.latlng;
                        
                        const locationData = await mockBackend.mapService.reverseGeocode(lat, lng);
                        setSelectedLocation(locationData);

                        if (marker) {
                            newMap.removeLayer(marker);
                        }

                        const newMarker = L.marker([lat, lng])
                            .addTo(newMap)
                            .bindPopup(`
                                <div class="map-marker-popup">
                                    <strong>${type === 'pickup' ? 'Pickup' : 'Destination'}</strong><br>
                                    ${locationData.address}
                                    <button onclick="
                                        const event = new CustomEvent('confirmLocation', { 
                                            detail: { 
                                                type: '${type}', 
                                                location: ${JSON.stringify(locationData).replace(/'/g, "\\'")}
                                            } 
                                        });
                                        window.dispatchEvent(event);
                                    ">
                                        Select This Location
                                    </button>
                                </div>
                            `)
                            .openPopup();

                        setMarker(newMarker);
                    };

                    newMap.on('click', clickHandler);
                }

                const handleConfirmLocation = (e) => {
                    if (e.detail.type === type) {
                        onLocationSelect(e.detail.location);
                        setSelectedLocation(e.detail.location);
                        NotificationSystem.showNotification(
                            'Location Set!',
                            `${type === 'pickup' ? 'Pickup' : 'Destination'} location confirmed: ${e.detail.location.address}`,
                            'success',
                            3000
                        );
                    }
                };

                window.addEventListener('confirmLocation', handleConfirmLocation);

                return () => {
                    window.removeEventListener('confirmLocation', handleConfirmLocation);
                };
            }, [map, type, defaultLocation, marker, onLocationSelect]);

            const useCurrentLocation = () => {
                if (navigator.geolocation) {
                    navigator.geolocation.getCurrentPosition(
                        async (position) => {
                            const { latitude, longitude } = position.coords;
                            
                            if (map) {
                                map.setView([latitude, longitude], 15);
                                
                                const locationData = await mockBackend.mapService.reverseGeocode(latitude, longitude);
                                setSelectedLocation(locationData);

                                if (marker) {
                                    map.removeLayer(marker);
                                }

                                const newMarker = L.marker([latitude, longitude])
                                    .addTo(map)
                                    .bindPopup(`
                                        <div class="map-marker-popup">
                                            <strong>Your Current Location</strong><br>
                                            ${locationData.address}
                                            <button onclick="
                                                const event = new CustomEvent('confirmLocation', { 
                                                    detail: { 
                                                        type: '${type}', 
                                                        location: ${JSON.stringify(locationData).replace(/'/g, "\\'")}
                                                    } 
                                                });
                                                window.dispatchEvent(event);
                                            ">
                                                Select This Location
                                            </button>
                                        </div>
                                    `)
                                    .openPopup();

                                setMarker(newMarker);
                            }
                        },
                        (error) => {
                            NotificationSystem.showNotification(
                                'Location Error',
                                'Unable to get your current location. Please enable location services.',
                                'error'
                            );
                        }
                    );
                } else {
                    NotificationSystem.showNotification(
                        'Not Supported',
                        'Geolocation is not supported by your browser.',
                        'error'
                    );
                }
            };

            const confirmLocation = () => {
                if (selectedLocation) {
                    onLocationSelect(selectedLocation);
                    NotificationSystem.showNotification(
                        'Location Set!',
                        `${type === 'pickup' ? 'Pickup' : 'Destination'} location confirmed: ${selectedLocation.address}`,
                        'success',
                        3000
                    );
                } else {
                    NotificationSystem.showNotification(
                        'Select Location',
                        'Please select a location on the map first.',
                        'warning'
                    );
                }
            };

            return (
                <div className="map-section">
                    <h3>
                        {type === 'pickup' ? '📍 Select Pickup Location' : '🎯 Select Destination'}
                    </h3>
                    <div id={`${type}Map`} style={{ height: '300px', borderRadius: '10px' }}></div>
                    
                    {selectedLocation && (
                        <div className="location-display">
                            <p><strong>Selected Location:</strong></p>
                            <p>{selectedLocation.address}</p>
                            <p>Lat: {selectedLocation.lat.toFixed(4)}, Lng: {selectedLocation.lng.toFixed(4)}</p>
                        </div>
                    )}
                    
                    <div className="map-controls">
                        <button className="use-current-location" onClick={useCurrentLocation}>
                            📍 Use Current Location
                        </button>
                        <button className="confirm-location" onClick={confirmLocation}>
                            ✅ Confirm Location
                        </button>
                    </div>
                </div>
            );
        };

        // Login Component
        const Login = ({ onSwitchToRegister }) => {
            const { login, loading } = useAuth();
            const [email, setEmail] = useState('');
            const [password, setPassword] = useState('');
            const [error, setError] = useState('');

            const handleSubmit = async (e) => {
                e.preventDefault();
                setError('');
                
                const result = await login(email, password);
                if (!result.success) {
                    setError(result.message);
                }
            };

            return (
                <div className="auth-container">
                    <div className="auth-form">
                        <div style={{ textAlign: 'center', marginBottom: '1rem' }}>
                            <AppLogo size="large" />
                        </div>
                        <h2>Login to Zoho Ride</h2>
                        {error && <div className="error-message">{error}</div>}
                        <form onSubmit={handleSubmit}>
                            <div className="form-group">
                                <label>Email:</label>
                                <input
                                    type="email"
                                    value={email}
                                    onChange={(e) => setEmail(e.target.value)}
                                    placeholder="Enter your email"
                                    required
                                />
                            </div>
                            <div className="form-group">
                                <label>Password:</label>
                                <input
                                    type="password"
                                    value={password}
                                    onChange={(e) => setPassword(e.target.value)}
                                    placeholder="Enter your password"
                                    required
                                />
                            </div>
                            <button type="submit" disabled={loading}>
                                {loading ? 'Logging in...' : 'Login'}
                            </button>
                        </form>
                        <div className="link-text">
                            Don't have an account? <a href="#" onClick={onSwitchToRegister}>Register here</a>
                        </div>
                        
                        <div style={{ marginTop: '2rem', padding: '1rem', background: '#f8f9fa', borderRadius: '8px', fontSize: '0.9rem' }}>
                            <strong>Demo Accounts:</strong><br/>
                            Rider: rider@example.com / password123<br/>
                            Driver: driver@example.com / password123
                        </div>
                    </div>
                </div>
            );
        };

        // Register Component
        const Register = ({ onSwitchToLogin }) => {
            const { register, loading } = useAuth();
            const [formData, setFormData] = useState({
                name: '',
                email: '',
                password: '',
                phone: '',
                userType: 'rider',
                vehicleType: '',
                vehicleNumber: ''
            });
            const [error, setError] = useState('');

            const handleChange = (e) => {
                setFormData({
                    ...formData,
                    [e.target.name]: e.target.value
                });
            };

            const handleSubmit = async (e) => {
                e.preventDefault();
                setError('');
                
                const result = await register(formData);
                if (!result.success) {
                    setError(result.message);
                }
            };

            return (
                <div className="auth-container">
                    <div className="auth-form">
                        <div style={{ textAlign: 'center', marginBottom: '1rem' }}>
                            <AppLogo size="large" />
                        </div>
                        <h2>Join Zoho Ride</h2>
                        {error && <div className="error-message">{error}</div>}
                        <form onSubmit={handleSubmit}>
                            <div className="form-group">
                                <label>Full Name:</label>
                                <input
                                    type="text"
                                    name="name"
                                    value={formData.name}
                                    onChange={handleChange}
                                    placeholder="Enter your full name"
                                    required
                                />
                            </div>
                            <div className="form-group">
                                <label>Email:</label>
                                <input
                                    type="email"
                                    name="email"
                                    value={formData.email}
                                    onChange={handleChange}
                                    placeholder="Enter your email"
                                    required
                                />
                            </div>
                            <div className="form-group">
                                <label>Password:</label>
                                <input
                                    type="password"
                                    name="password"
                                    value={formData.password}
                                    onChange={handleChange}
                                    placeholder="Create a password"
                                    required
                                />
                            </div>
                            <div className="form-group">
                                <label>Phone:</label>
                                <input
                                    type="tel"
                                    name="phone"
                                    value={formData.phone}
                                    onChange={handleChange}
                                    placeholder="Enter your phone number"
                                    required
                                />
                            </div>
                            <div className="form-group">
                                <label>I am a:</label>
                                <select name="userType" value={formData.userType} onChange={handleChange}>
                                    <option value="rider">Rider</option>
                                    <option value="driver">Driver</option>
                                </select>
                            </div>
                            {formData.userType === 'driver' && (
                                <>
                                    <div className="form-group">
                                        <label>Vehicle Type:</label>
                                        <select name="vehicleType" value={formData.vehicleType} onChange={handleChange} required>
                                            <option value="">Select Vehicle Type</option>
                                            <option value="Bike">Bike</option>
                                            <option value="Auto">Auto Rickshaw</option>
                                            <option value="Car">Car</option>
                                        </select>
                                    </div>
                                    <div className="form-group">
                                        <label>Vehicle Number:</label>
                                        <input
                                            type="text"
                                            name="vehicleNumber"
                                            value={formData.vehicleNumber}
                                            onChange={handleChange}
                                            placeholder="e.g., KA01AB1234"
                                            required
                                        />
                                    </div>
                                </>
                            )}
                            <button type="submit" disabled={loading}>
                                {loading ? 'Creating Account...' : 'Register'}
                            </button>
                        </form>
                        <div className="link-text">
                            Already have an account? <a href="#" onClick={onSwitchToLogin}>Login here</a>
                        </div>
                    </div>
                </div>
            );
        };

        // Enhanced Rider Dashboard with Ride History
        const RiderDashboard = () => {
            const { user, logout } = useAuth();
            const [pickupLocation, setPickupLocation] = useState(null);
            const [destination, setDestination] = useState(null);
            const [currentRide, setCurrentRide] = useState(null);
            const [message, setMessage] = useState('');

            useEffect(() => {
                // Check for existing ride
                const ride = mockBackend.getCurrentRide();
                if (ride) {
                    setCurrentRide(ride);
                }

                // Listen for ride acceptance
                const handleRideAccepted = (data) => {
                    const ride = mockBackend.getRideById(data.rideId);
                    if (ride) {
                        setCurrentRide(ride);
                        NotificationSystem.showNotification(
                            'Ride Accepted! 🎉',
                            `Your ride has been accepted by ${data.driver.name} (${data.driver.vehicleType})`,
                            'success'
                        );
                    }
                };

                // Listen for status updates
                const handleStatusUpdate = (data) => {
                    if (data.rideId === currentRide?.id) {
                        setCurrentRide(data.ride);
                        NotificationSystem.showNotification(
                            'Status Updated',
                            `Ride status: ${data.status}`,
                            'info'
                        );
                    }
                };

                eventBus.on('rideAccepted', handleRideAccepted);
                eventBus.on('rideStatusUpdate', handleStatusUpdate);

                return () => {
                    eventBus.off('rideAccepted', handleRideAccepted);
                    eventBus.off('rideStatusUpdate', handleStatusUpdate);
                };
            }, [currentRide]);

            const requestRide = async () => {
                if (!pickupLocation || !destination) {
                    NotificationSystem.showNotification(
                        'Select Locations',
                        'Please select both pickup and destination locations',
                        'warning'
                    );
                    return;
                }

                setMessage('Finding you a ride...');
                try {
                    const ride = await mockBackend.requestRide(pickupLocation, destination);
                    setCurrentRide(ride);
                    setMessage('🚗 Ride requested! Finding nearby drivers...');
                    
                    NotificationSystem.showNotification(
                        'Ride Requested!',
                        'We are finding the best driver for you...',
                        'success'
                    );
                } catch (error) {
                    setMessage('Error requesting ride: ' + error.message);
                    NotificationSystem.showNotification(
                        'Request Failed',
                        error.message,
                        'error'
                    );
                }
            };

            const cancelRide = async () => {
                try {
                    await mockBackend.updateRideStatus(currentRide.id, 'cancelled');
                    setCurrentRide(null);
                    setMessage('Ride cancelled successfully');
                    NotificationSystem.showNotification(
                        'Ride Cancelled',
                        'Your ride has been cancelled',
                        'info'
                    );
                } catch (error) {
                    setMessage('Error cancelling ride: ' + error.message);
                    NotificationSystem.showNotification(
                        'Cancellation Failed',
                        error.message,
                        'error'
                    );
                }
            };

            const completeRide = async () => {
                try {
                    await mockBackend.updateRideStatus(currentRide.id, 'completed');
                    setCurrentRide(null);
                    setMessage('✅ Ride completed! Thank you for using Zoho Ride.');
                    NotificationSystem.showNotification(
                        'Ride Completed!',
                        'Thank you for choosing Zoho Ride!',
                        'success'
                    );
                } catch (error) {
                    setMessage('Error completing ride: ' + error.message);
                    NotificationSystem.showNotification(
                        'Completion Failed',
                        error.message,
                        'error'
                    );
                }
            };

            return (
                <div className="dashboard">
                    <header className="dashboard-header">
                        <AppLogo />
                        <div className="user-info">
                            <span>Welcome, {user?.name}</span>
                            <button className="logout-btn" onClick={logout}>Logout</button>
                        </div>
                    </header>

                    <div className="dashboard-content">
                        {message && (
                            <div className={message.includes('Error') ? 'error-message' : 'success-message'}>
                                {message}
                            </div>
                        )}

                        {!currentRide ? (
                            <>
                                <div className="map-container">
                                    <MapSelector 
                                        type="pickup"
                                        onLocationSelect={setPickupLocation}
                                        defaultLocation={[12.9716, 77.5946]}
                                    />
                                    <MapSelector 
                                        type="destination"
                                        onLocationSelect={setDestination}
                                        defaultLocation={[12.9250, 77.6250]}
                                    />
                                </div>

                                <div className="ride-booking">
                                    <h2>Book a Ride</h2>
                                    
                                    {pickupLocation && destination && (
                                        <div className="ride-info">
                                            <p><strong>Route Details:</strong></p>
                                            <p>📍 From: {pickupLocation.address}</p>
                                            <p>🎯 To: {destination.address}</p>
                                            <p>📏 Distance: {mockBackend.mapService.calculateDistance(
                                                pickupLocation.lat, pickupLocation.lng,
                                                destination.lat, destination.lng
                                            ).toFixed(2)} km</p>
                                            <p>💰 Estimated Fare: ₹{mockBackend.mapService.calculateFare(
                                                mockBackend.mapService.calculateDistance(
                                                    pickupLocation.lat, pickupLocation.lng,
                                                    destination.lat, destination.lng
                                                )
                                            )}</p>
                                        </div>
                                    )}

                                    <button 
                                        className="request-btn" 
                                        onClick={requestRide}
                                        disabled={!pickupLocation || !destination}
                                    >
                                        🚗 Request Ride
                                    </button>
                                </div>
                            </>
                        ) : (
                            <div className="ride-status">
                                <h2>Ride Status</h2>
                                <div className="status-badge">
                                    {currentRide.status.toUpperCase()}
                                </div>
                                
                                <div className="ride-info">
                                    <p><strong>📍 From:</strong> {currentRide.pickupLocation.address}</p>
                                    <p><strong>🎯 To:</strong> {currentRide.destination.address}</p>
                                    <p><strong>💰 Fare:</strong> ₹{currentRide.fare}</p>
                                    <p><strong>📏 Distance:</strong> {currentRide.distance} km</p>
                                    <p><strong>⏱️ Duration:</strong> {currentRide.duration} min</p>
                                    
                                    {currentRide.driverName && (
                                        <>
                                            <p><strong>👨‍💼 Driver:</strong> {currentRide.driverName}</p>
                                            <p><strong>🚗 Vehicle:</strong> {currentRide.vehicleType} - {currentRide.vehicleNumber}</p>
                                            <p><strong>📞 Phone:</strong> {currentRide.driverPhone}</p>
                                        </>
                                    )}
                                </div>

                                {currentRide.status === 'requested' && (
                                    <button className="cancel-btn" onClick={cancelRide}>
                                        Cancel Ride
                                    </button>
                                )}

                                {currentRide.status === 'ongoing' && (
                                    <button className="request-btn" onClick={completeRide}>
                                        Complete Ride
                                    </button>
                                )}
                            </div>
                        )}

                        {/* Ride History for Rider */}
                        <RideHistory userType="rider" />
                    </div>
                </div>
            );
        };

        // Enhanced Driver Dashboard with Ride History
        const DriverDashboard = () => {
            const { user, logout } = useAuth();
            const [isAvailable, setIsAvailable] = useState(false);
            const [currentRide, setCurrentRide] = useState(null);
            const [rideRequests, setRideRequests] = useState([]);
            const [message, setMessage] = useState('');
            const [newRideNotification, setNewRideNotification] = useState(null);

            useEffect(() => {
                // Check for existing ride
                const ride = mockBackend.getCurrentRide();
                if (ride) {
                    setCurrentRide(ride);
                }

                // Load ride requests
                const loadRideRequests = () => {
                    const requests = mockBackend.getRideRequests();
                    console.log('Loading ride requests:', requests);
                    setRideRequests(requests);
                };

                loadRideRequests();

                // Listen for new ride requests
                const handleNewRide = (data) => {
                    console.log('New ride received in driver dashboard:', data);
                    const updatedRequests = mockBackend.getRideRequests();
                    setRideRequests(updatedRequests);
                    
                    // Show prominent notification
                    setNewRideNotification(data.ride);
                    
                    NotificationSystem.showNotification(
                        '🚗 New Ride Request!',
                        `From: ${data.ride.pickupLocation.address} | To: ${data.ride.destination.address} | Fare: ₹${data.ride.fare}`,
                        'warning',
                        10000
                    );

                    // Auto-remove notification after 10 seconds
                    setTimeout(() => {
                        setNewRideNotification(null);
                    }, 10000);
                };

                // Listen for status updates
                const handleStatusUpdate = (data) => {
                    if (data.rideId === currentRide?.id) {
                        setCurrentRide(data.ride);
                        NotificationSystem.showNotification(
                            'Status Updated',
                            `Ride status: ${data.status}`,
                            'info'
                        );
                    }
                    
                    // Refresh ride requests when status changes
                    const updatedRequests = mockBackend.getRideRequests();
                    setRideRequests(updatedRequests);
                };

                eventBus.on('driverNotification', handleNewRide);
                eventBus.on('rideStatusUpdate', handleStatusUpdate);

                // Set up periodic refresh
                const interval = setInterval(() => {
                    if (isAvailable && !currentRide) {
                        loadRideRequests();
                    }
                }, 3000);

                return () => {
                    eventBus.off('driverNotification', handleNewRide);
                    eventBus.off('rideStatusUpdate', handleStatusUpdate);
                    clearInterval(interval);
                };
            }, [isAvailable, currentRide]);

            const toggleAvailability = () => {
                const newAvailability = !isAvailable;
                setIsAvailable(newAvailability);
                mockBackend.updateDriverAvailability(user.id, newAvailability);
                
                // Refresh ride requests when going online
                if (newAvailability) {
                    const requests = mockBackend.getRideRequests();
                    setRideRequests(requests);
                }
                
                NotificationSystem.showNotification(
                    newAvailability ? 'You are now online!' : 'You are now offline',
                    newAvailability ? 'Ready to accept ride requests' : 'No longer accepting rides',
                    newAvailability ? 'success' : 'info'
                );
            };

            const acceptRide = async (rideId) => {
                try {
                    const ride = await mockBackend.acceptRide(rideId);
                    setCurrentRide(ride);
                    
                    // Update ride requests list
                    const updatedRequests = mockBackend.getRideRequests();
                    setRideRequests(updatedRequests);
                    
                    setNewRideNotification(null);
                    setMessage('✅ Ride accepted successfully!');
                    
                    NotificationSystem.showNotification(
                        'Ride Accepted!',
                        `You are now driving to ${ride.pickupLocation.address}`,
                        'success'
                    );
                } catch (error) {
                    setMessage('Error accepting ride: ' + error.message);
                    NotificationSystem.showNotification(
                        'Acceptance Failed',
                        error.message,
                        'error'
                    );
                }
            };

            const updateRideStatus = async (status) => {
                try {
                    const ride = await mockBackend.updateRideStatus(currentRide.id, status);
                    setCurrentRide(ride);
                    
                    if (status === 'completed') {
                        setCurrentRide(null);
                        setIsAvailable(true);
                        setMessage('✅ Ride completed successfully!');
                        
                        // Refresh ride requests
                        const requests = mockBackend.getRideRequests();
                        setRideRequests(requests);
                        
                        NotificationSystem.showNotification(
                            'Ride Completed!',
                            'Ready for your next ride',
                            'success'
                        );
                    }
                } catch (error) {
                    setMessage('Error updating ride status: ' + error.message);
                    NotificationSystem.showNotification(
                        'Update Failed',
                        error.message,
                        'error'
                    );
                }
            };

            return (
                <div className="dashboard">
                    <header className="dashboard-header">
                        <AppLogo />
                        <div className="user-info">
                            <span>Welcome, {user?.name}</span>
                            <button className="logout-btn" onClick={logout}>Logout</button>
                        </div>
                    </header>

                    <div className="dashboard-content">
                        {message && (
                            <div className={message.includes('Error') ? 'error-message' : 'success-message'}>
                                {message}
                            </div>
                        )}

                        {/* New Ride Notification Banner */}
                        {newRideNotification && (
                            <div className="new-ride-notification">
                                <h3>🚗 New Ride Request!</h3>
                                <p><strong>From:</strong> {newRideNotification.pickupLocation.address}</p>
                                <p><strong>To:</strong> {newRideNotification.destination.address}</p>
                                <p><strong>Fare:</strong> ₹{newRideNotification.fare}</p>
                                <button 
                                    className="accept-btn"
                                    onClick={() => acceptRide(newRideNotification.id)}
                                    style={{ marginTop: '10px', background: 'white', color: '#ee5a24' }}
                                >
                                    ✅ Accept This Ride
                                </button>
                            </div>
                        )}

                        <div className="availability-toggle">
                            <button 
                                className={`toggle-btn ${isAvailable ? 'available' : 'unavailable'}`}
                                onClick={toggleAvailability}
                            >
                                {isAvailable ? '🚫 Go Offline' : '✅ Go Online'}
                            </button>
                            <span>Status: {isAvailable ? '🟢 Available' : '🔴 Offline'}</span>
                        </div>

                        {currentRide ? (
                            <div className="current-ride">
                                <h2>Current Ride</h2>
                                <div className="ride-info">
                                    <p><strong>📍 From:</strong> {currentRide.pickupLocation.address}</p>
                                    <p><strong>🎯 To:</strong> {currentRide.destination.address}</p>
                                    <p><strong>👤 Rider:</strong> {currentRide.riderName}</p>
                                    <p><strong>📞 Rider Phone:</strong> {currentRide.riderPhone}</p>
                                    <p><strong>💰 Fare:</strong> ₹{currentRide.fare}</p>
                                    <p><strong>📏 Distance:</strong> {currentRide.distance} km</p>
                                    <p><strong>⏱️ Duration:</strong> {currentRide.duration} min</p>
                                    <p><strong>Status:</strong> {currentRide.status}</p>
                                </div>
                                
                                <div className="ride-actions">
                                    {currentRide.status === 'accepted' && (
                                        <button onClick={() => updateRideStatus('arrived')}>
                                            🚗 Mark as Arrived
                                        </button>
                                    )}
                                    {currentRide.status === 'arrived' && (
                                        <button onClick={() => updateRideStatus('ongoing')}>
                                            🚀 Start Ride
                                        </button>
                                    )}
                                    {currentRide.status === 'ongoing' && (
                                        <button onClick={() => updateRideStatus('completed')}>
                                            ✅ Complete Ride
                                        </button>
                                    )}
                                </div>
                            </div>
                        ) : (
                            <div className="ride-requests">
                                <h2>Available Ride Requests</h2>
                                {!isAvailable ? (
                                    <p>Go online to see ride requests</p>
                                ) : rideRequests.length === 0 ? (
                                    <p>No ride requests available at the moment</p>
                                ) : (
                                    rideRequests.map(request => (
                                        <div key={request.id} className="ride-request-card">
                                            <h4>🚗 Ride Request</h4>
                                            <p><strong>📍 From:</strong> {request.pickupLocation.address}</p>
                                            <p><strong>🎯 To:</strong> {request.destination.address}</p>
                                            <p><strong>👤 Rider:</strong> {request.riderName}</p>
                                            <p><strong>💰 Fare:</strong> ₹{request.fare}</p>
                                            <p><strong>📏 Distance:</strong> {request.distance} km</p>
                                            <p><strong>⏱️ Duration:</strong> {request.duration} min</p>
                                            <button 
                                                className="accept-btn"
                                                onClick={() => acceptRide(request.id)}
                                            >
                                                ✅ Accept Ride
                                            </button>
                                        </div>
                                    ))
                                )}
                            </div>
                        )}

                        {/* Ride History for Driver */}
                        <RideHistory userType="driver" />
                    </div>
                </div>
            );
        };

        // Main App Component
        const App = () => {
            const { user } = useAuth();
            const [currentView, setCurrentView] = useState('login');

            if (user) {
                return user.userType === 'driver' ? <DriverDashboard /> : <RiderDashboard />;
            }

            return currentView === 'login' ? (
                <Login onSwitchToRegister={() => setCurrentView('register')} />
            ) : (
                <Register onSwitchToLogin={() => setCurrentView('login')} />
            );
        };

        // Render the app
        const root = ReactDOM.createRoot(document.getElementById('root'));
        root.render(
            <AuthProvider>
                <div className="app-container">
                    <App />
                </div>
            </AuthProvider>
        );
    </script>
</body>
</html>
