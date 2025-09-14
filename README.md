<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>LocalFinder API Documentation ✨</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Chosen Palette: Calm Harmony (Light neutrals with indigo accent) -->
    <!-- Application Structure Plan: A three-column interactive layout. Left sidebar for sticky navigation of services/endpoints. Middle column for detailed descriptions. Right sidebar for sticky code examples (request/response). This structure is chosen because it's a standard, highly usable pattern for API documentation, allowing developers to quickly navigate and cross-reference information without scrolling. Gemini features (Search, Explain, Code Gen) are added to enhance developer productivity within this structure. -->
    <!-- Visualization & Content Choices: The core "visualization" is the information architecture itself. Report Info: API structure -> Goal: Organize/Navigate -> Presentation: Hierarchical sidebar -> Interaction: Click to show content -> Justification: Provides rapid, non-linear access to technical specs. Report Info: Code examples -> Goal: Inform -> Presentation: Dedicated code blocks -> Interaction: Copy to clipboard -> Justification: Improves developer workflow. Gemini API is used for NL->Search, NL->Explanation, and Spec->Code generation. NO SVG/Mermaid used. -->
    <!-- CONFIRMATION: NO SVG graphics used. NO Mermaid JS used. -->
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap');
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f8fafc; /* slate-50 */
        }
        .content-panel, .code-panel { display: none; }
        .content-panel.active, .code-panel.active { display: block; }
        .nav-link.active {
            background-color: #eef2ff; /* indigo-100 */
            color: #4338ca; /* indigo-700 */
            font-weight: 600;
        }
        .nav-link.highlight {
            background-color: #c7d2fe; /* indigo-200 */
            transition: background-color 0.3s ease-in-out;
        }
        .nav-group-title.active { color: #4f46e5; /* indigo-600 */ }
        .method-badge {
            font-size: 0.8rem; font-weight: 700;
            padding: 0.125rem 0.5rem; border-radius: 0.25rem;
            text-transform: uppercase;
        }
        .method-get { background-color: #dbeafe; color: #1d4ed8; } /* blue */
        .method-post { background-color: #dcfce7; color: #166534; } /* green */
        .method-put { background-color: #ffedd5; color: #9a3412; } /* orange */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f5f9; }
        ::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #94a3b8; }
        .loader {
            border: 2px solid #f3f3f3; border-top: 2px solid #4f46e5;
            border-radius: 50%; width: 16px; height: 16px;
            animation: spin 1s linear infinite;
        }
        @keyframes spin { 0% { transform: rotate(0deg); } 100% { transform: rotate(360deg); } }
    </style>
</head>
<body class="text-slate-800">

    <!-- Explanation Modal -->
    <div id="explanation-modal" class="fixed inset-0 bg-black bg-opacity-50 z-50 hidden items-center justify-center">
        <div class="bg-white rounded-lg shadow-xl p-6 w-11/12 max-w-2xl transform transition-all">
            <div class="flex justify-between items-center mb-4">
                <h3 class="text-xl font-bold text-slate-900">✨ AI Explanation</h3>
                <button id="close-modal-btn" class="text-slate-500 hover:text-slate-800">&times;</button>
            </div>
            <div id="modal-content" class="text-slate-700 max-h-[60vh] overflow-y-auto pr-2">
                <!-- AI content will be injected here -->
            </div>
        </div>
    </div>

    <div class="container mx-auto">
        <div class="lg:grid lg:grid-cols-12 lg:gap-8">
            <aside id="api-nav-container" class="lg:col-span-3 lg:sticky top-0 h-screen overflow-y-auto py-8 px-4 bg-white border-r border-slate-200">
                <h1 class="text-2xl font-bold text-slate-900 mb-2">LocalFinder API</h1>
                <p class="text-sm text-slate-500 mb-4">Version 1.0</p>
                <div class="relative mb-4">
                    <input type="text" id="ai-search" placeholder="✨ Ask AI: 'How to log in?'" class="w-full pl-8 pr-3 py-2 border border-slate-300 rounded-md text-sm focus:ring-2 focus:ring-indigo-500 focus:border-indigo-500">
                    <div class="absolute inset-y-0 left-0 pl-2.5 flex items-center pointer-events-none">
                        <svg class="h-4 w-4 text-slate-400" fill="none" viewBox="0 0 24 24" stroke="currentColor"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z"></path></svg>
                    </div>
                     <div id="search-loader" class="absolute inset-y-0 right-0 pr-2.5 hidden items-center"><div class="loader"></div></div>
                </div>
                <nav id="api-nav"></nav>
            </aside>

            <main id="api-content-container" class="lg:col-span-5 py-8 px-4">
                <!-- Content will be injected here -->
            </main>

            <aside id="api-code-container" class="lg:col-span-4 lg:sticky top-0 h-screen overflow-y-auto py-8 px-4 bg-slate-100 rounded-lg">
                 <!-- Code examples will be injected here -->
            </aside>
        </div>
    </div>

<script>
const apiData = {
    introduction: {
        id: 'introduction',
        title: 'Introduction',
        content: `
            <h2 class="text-3xl font-bold mb-4 text-slate-900">Getting Started</h2>
            <p class="text-slate-600 mb-4">This document outlines the RESTful API contracts for the LocalFinder microservices ecosystem. It defines the endpoints, request/response formats, authentication mechanisms, and data models required to build the features specified in the Product Specification Document v1.0.</p>
            
            <div class="space-y-6">
                <div>
                    <h3 class="text-xl font-semibold mb-2 text-slate-800">Base URL</h3>
                    <p class="text-slate-600 mb-2">All API endpoints are prefixed with the following base URL:</p>
                    <pre class="bg-slate-900 text-white p-3 rounded-md text-sm"><code>https://api.localfinder.app/v1</code></pre>
                </div>
                <div>
                    <h3 class="text-xl font-semibold mb-2 text-slate-800">Authentication</h3>
                    <p class="text-slate-600 mb-2">The API uses JSON Web Tokens (JWT) for authenticating requests. Once a user logs in, a JWT is issued. This token must be included in the <code class="bg-slate-200 p-1 rounded">Authorization</code> header for all protected endpoints.</p>
                    <pre class="bg-slate-900 text-white p-3 rounded-md text-sm"><code>Authorization: Bearer <jwt_token></code></pre>
                </div>
                <div>
                    <h3 class="text-xl font-semibold mb-2 text-slate-800">Pagination</h3>
                    <p class="text-slate-600 mb-2">For endpoints that return a list of items, use the <code class="bg-slate-200 p-1 rounded">page</code> and <code class="bg-slate-200 p-1 rounded">limit</code> query parameters.</p>
                </div>
            </div>
        `,
        code: `
            <h3 class="text-lg font-semibold mb-2 text-slate-800">Example Error Response</h3>
            <p class="text-sm text-slate-600 mb-2">Standard HTTP status codes are used. The response body for an error will contain a JSON object with more details.</p>
            <div class="relative">
                <button class="copy-btn absolute top-2 right-2 bg-slate-700 text-white text-xs px-2 py-1 rounded">Copy</button>
                <pre class="bg-slate-800 text-white p-4 rounded-md text-sm overflow-x-auto"><code class="language-json">${JSON.stringify({
                  "error": {
                    "code": "VALIDATION_ERROR",
                    "message": "The 'email' field must be a valid email address.",
                    "details": [
                      { "field": "email", "issue": "Invalid format" }
                    ]
                  }
                }, null, 2)}</code></pre>
            </div>
             <h3 class="text-lg font-semibold mb-2 mt-4 text-slate-800">Pagination Response</h3>
            <div class="relative">
                <button class="copy-btn absolute top-2 right-2 bg-slate-700 text-white text-xs px-2 py-1 rounded">Copy</button>
                <pre class="bg-slate-800 text-white p-4 rounded-md text-sm overflow-x-auto"><code class="language-json">${JSON.stringify({
                  "data": [ "..." ],
                  "pagination": {
                    "currentPage": 1,
                    "totalPages": 10,
                    "totalItems": 198,
                    "hasNext": true,
                    "hasPrev": false
                  }
                }, null, 2)}</code></pre>
            </div>
        `
    },
    dataModels: {
        id: 'data-models',
        title: 'Data Models',
        content: `
            <h2 class="text-3xl font-bold mb-4 text-slate-900">Data Models</h2>
            <p class="text-slate-600 mb-4">Simplified data models for key entities in the LocalFinder ecosystem.</p>
        `,
        code: `
            <div class="space-y-4">
                <div>
                    <h3 class="text-lg font-semibold mb-2 text-slate-800">User</h3>
                    <div class="relative">
                        <button class="copy-btn absolute top-2 right-2 bg-slate-700 text-white text-xs px-2 py-1 rounded">Copy</button>
                        <pre class="bg-slate-800 text-white p-4 rounded-md text-sm overflow-x-auto"><code class="language-json">${JSON.stringify({
                            userId: "usr_1a2b3c",
                            name: "Priya Sharma",
                            email: "priya@example.com",
                            phone: "+919876543210",
                            roles: ["USER"],
                            createdAt: "2025-09-14T15:00:00Z"
                        }, null, 2)}</code></pre>
                    </div>
                </div>
                <div>
                    <h3 class="text-lg font-semibold mb-2 text-slate-800">Business</h3>
                    <div class="relative">
                        <button class="copy-btn absolute top-2 right-2 bg-slate-700 text-white text-xs px-2 py-1 rounded">Copy</button>
                        <pre class="bg-slate-800 text-white p-4 rounded-md text-sm overflow-x-auto"><code class="language-json">${JSON.stringify({
                            businessId: "biz_4d5e6f",
                            ownerId: "usr_7g8h9i",
                            businessName: "Artisan Beans Cafe",
                            status: "VERIFIED",
                            createdAt: "2025-09-12T10:00:00Z"
                        }, null, 2)}</code></pre>
                    </div>
                </div>
                <div>
                    <h3 class="text-lg font-semibold mb-2 text-slate-800">Listing</h3>
                    <div class="relative">
                        <button class="copy-btn absolute top-2 right-2 bg-slate-700 text-white text-xs px-2 py-1 rounded">Copy</button>
                        <pre class="bg-slate-800 text-white p-4 rounded-md text-sm overflow-x-auto"><code class="language-json">${JSON.stringify({
                            listingId: "lst_xyz789",
                            businessId: "biz_4d5e6f",
                            title: "Artisan Beans Cafe",
                            category: "Cafe",
                            location: {
                                type: "Point",
                                coordinates: [78.476, 17.366]
                            },
                            rating: { average: 4.8, count: 125 }
                        }, null, 2)}</code></pre>
                    </div>
                </div>
            </div>
        `
    },
    services: [
        {
            name: 'Auth Service',
            basePath: '/auth',
            endpoints: [
                {
                    id: 'auth-register',
                    method: 'POST',
                    path: '/register',
                    description: 'Registers a new user (customer or business owner). An OTP is sent to the provided phone number for verification.',
                    auth: 'Not Required',
                    request: {
                        name: "Anjali Verma",
                        email: "anjali@boutique.com",
                        password: "strong-password-123",
                        phone: "+919988776655",
                        role: "BUSINESS_OWNER"
                    },
                    response: {
                        message: "OTP sent to your phone for verification."
                    }
                },
                {
                    id: 'auth-login',
                    method: 'POST',
                    path: '/login',
                    description: 'Authenticates a user with email and password, returning a JWT.',
                    auth: 'Not Required',
                    request: {
                        email: "priya@example.com",
                        password: "her-password"
                    },
                    response: {
                        accessToken: "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...",
                        user: { userId: "usr_1a2b3c", name: "Priya Sharma", "..." : "..."}
                    }
                },
                {
                    id: 'auth-verify',
                    method: 'POST',
                    path: '/verify-otp',
                    description: 'Verifies the OTP sent during registration to activate the account.',
                    auth: 'Not Required',
                    request: {
                        phone: "+919988776655",
                        otp: "123456"
                    },
                    response: { message: "Returns User object and JWT, logging the user in."}
                },
                {
                    id: 'auth-me',
                    method: 'GET',
                    path: '/me',
                    description: 'Retrieves the profile of the currently authenticated user.',
                    auth: 'Required',
                    request: null,
                    response: { userId: "usr_1a2b3c", name: "Priya Sharma", "..." : "..."}
                }
            ]
        },
        {
            name: 'Listings Service',
            basePath: '/listings',
            endpoints: [
                {
                    id: 'listings-search',
                    method: 'GET',
                    path: '/search',
                    description: 'Core endpoint to search for listings based on location and query. Supports filtering and sorting.',
                    auth: 'Not Required',
                    parameters: [
                        { name: 'q', type: 'string', desc: 'Search query (e.g., "coffee").' },
                        { name: 'lat', type: 'float', desc: 'Latitude of search center.' },
                        { name: 'lon', type: 'float', desc: 'Longitude of search center.' },
                        { name: 'radius', type: 'int', desc: 'Search radius in meters.' },
                        { name: 'filters', type: 'string', desc: 'Comma-separated filters (e.g., "open_now").' },
                    ],
                    request: null,
                    response: {
                        data: [{ listingId: "lst_xyz789", title: "Artisan Beans Cafe", "...": "..." }],
                        pagination: { currentPage: 1, totalPages: 10, "...": "..." }
                    }
                },
                {
                    id: 'listings-get-one',
                    method: 'GET',
                    path: '/:id',
                    description: 'Retrieves a single listing by its ID.',
                    auth: 'Not Required',
                    request: null,
                    response: { listingId: "lst_xyz789", title: "Artisan Beans Cafe", "...": "..." }
                },
                {
                    id: 'listings-create',
                    method: 'POST',
                    path: '',
                    description: 'Creates a new business listing. User must be authenticated as a BUSINESS_OWNER.',
                    auth: 'Required (BUSINESS_OWNER)',
                    request: { title: "My New Cafe", category: "Cafe", "...": "..." },
                    response: { listingId: "lst_new123", title: "My New Cafe", "...": "..." }
                },
                 {
                    id: 'listings-add-review',
                    method: 'POST',
                    path: '/:id/reviews',
                    description: 'Adds a review to a specific listing.',
                    auth: 'Required (USER)',
                    request: { rating: 5, comment: "Amazing coffee and great ambiance!" },
                    response: { reviewId: "rev_abc456", userId: "usr_1a2b3c", rating: 5, "...": "..." }
                }
            ]
        },
        {
            name: 'Geo Service',
            basePath: '/geo',
            endpoints: [
                {
                    id: 'geo-route',
                    method: 'POST',
                    path: '/route',
                    description: 'Gets routing information between two points. This is a wrapper around the Google Directions API.',
                    auth: 'Required',
                    request: {
                        origin: { "lat": 17.366, "lon": 78.476 },
                        destination: { "lat": 17.437, "lon": 78.448 },
                        mode: "walking"
                    },
                    response: {
                        summary: { distance: "5.2 km", duration: "18 mins" },
                        polyline: "encoded_polyline_string_from_google",
                        steps: [{ instruction: "Head north on Road No. 1", "...": "..."}]
                    }
                }
            ]
        },
        {
            name: 'Orders Service',
            basePath: '/orders',
            endpoints: [
                 {
                    id: 'orders-create',
                    method: 'POST',
                    path: '',
                    description: 'Creates a new order (e.g., booking a service, purchasing a product).',
                    auth: 'Required',
                    request: {
                        listingId: "lst_xyz789",
                        items: [{ productId: "prod_123", quantity: 2, price: 150 }],
                        totalAmount: 300,
                        paymentMethod: "UPI"
                    },
                    response: { orderId: "ord_def789", status: "PLACED", "...": "..." }
                },
                {
                    id: 'orders-get-history',
                    method: 'GET',
                    path: '',
                    description: "Retrieves the authenticated user's order history.",
                    auth: 'Required',
                    request: null,
                    response: { data: [{ orderId: "ord_def789", status: "PLACED", "...": "..." }], pagination: { "...": "..."} }
                },
                 {
                    id: 'orders-cancel',
                    method: 'PUT',
                    path: '/:id/cancel',
                    description: 'Cancels an order. The API will enforce business logic (e.g., 3-minute cancellation window).',
                    auth: 'Required',
                    request: null,
                    response: { orderId: "ord_def789", status: "CANCELLED", "...": "..." }
                }
            ]
        },
    ]
};

document.addEventListener('DOMContentLoaded', () => {
    const navContainer = document.getElementById('api-nav');
    const contentContainer = document.getElementById('api-content-container');
    const codeContainer = document.getElementById('api-code-container');
    const searchInput = document.getElementById('ai-search');
    const searchLoader = document.getElementById('search-loader');
    const modal = document.getElementById('explanation-modal');
    const closeModalBtn = document.getElementById('close-modal-btn');
    const modalContent = document.getElementById('modal-content');

    const apiKey = ""; // Left empty as per instructions
    const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-05-20:generateContent?key=${apiKey}`;

    const endpointListForAI = apiData.services.flatMap(service => 
        service.endpoints.map(endpoint => `id: ${endpoint.id}, description: ${endpoint.description}, path: ${service.basePath}${endpoint.path || ''}`)
    ).join('; ');

    // Gemini API call helper
    async function callGemini(prompt, retries = 3, delay = 1000) {
        const payload = { contents: [{ parts: [{ text: prompt }] }] };
        try {
            const response = await fetch(apiUrl, {
                method: 'POST',
                headers: { 'Content-Type': 'application/json' },
                body: JSON.stringify(payload)
            });
            if (!response.ok) throw new Error(`HTTP error! status: ${response.status}`);
            const result = await response.json();
            const text = result.candidates?.[0]?.content?.parts?.[0]?.text;
            return text || "Sorry, I couldn't process that request.";
        } catch (error) {
            if (retries > 0) {
                await new Promise(res => setTimeout(res, delay));
                return callGemini(prompt, retries - 1, delay * 2);
            }
            console.error("Error calling Gemini API:", error);
            return "An error occurred while contacting the AI. Please try again later.";
        }
    }

    function parseSimpleMarkdown(text) {
        return text
            .replace(/\*\*(.*?)\*\*/g, '<strong>$1</strong>') // Bold
            .replace(/\n/g, '<br>'); // Newlines
    }

    let navHtml = '<ul class="space-y-4">';

    navHtml += `<li><a href="#" class="nav-link text-slate-600 hover:text-slate-900 block py-1" data-target="introduction">${apiData.introduction.title}</a></li>`;
    navHtml += `<li><a href="#" class="nav-link text-slate-600 hover:text-slate-900 block py-1" data-target="data-models">${apiData.dataModels.title}</a></li>`;

    contentContainer.innerHTML += `<div class="content-panel" id="content-introduction">${apiData.introduction.content}</div>`;
    codeContainer.innerHTML += `<div class="code-panel" id="code-introduction">${apiData.introduction.code}</div>`;
    contentContainer.innerHTML += `<div class="content-panel" id="content-data-models">${apiData.dataModels.content}</div>`;
    codeContainer.innerHTML += `<div class="code-panel" id="code-data-models">${apiData.dataModels.code}</div>`;
    
    apiData.services.forEach(service => {
        navHtml += `<li><h3 class="nav-group-title text-sm font-semibold uppercase text-slate-500 tracking-wider">${service.name}</h3><ul class="mt-2 space-y-1 border-l border-slate-200 pl-4">`;
        service.endpoints.forEach(endpoint => {
            const methodColorClasses = { 'GET': 'method-get', 'POST': 'method-post', 'PUT': 'method-put' };
            navHtml += `<li><a href="#" class="nav-link text-slate-600 hover:text-slate-900 text-sm flex items-center p-1 rounded-md" data-target="${endpoint.id}"><span class="method-badge ${methodColorClasses[endpoint.method]}">${endpoint.method}</span><span class="ml-2 truncate">${service.basePath}${endpoint.path || ''}</span></a></li>`;
            
            let contentHtml = `<div class="content-panel" id="content-${endpoint.id}">
                <div class="flex items-center justify-between gap-3 mb-2">
                    <div class="flex items-center gap-3">
                        <span class="method-badge ${methodColorClasses[endpoint.method]}">${endpoint.method}</span>
                        <h2 class="text-2xl font-semibold text-slate-800 break-all">${service.basePath}${endpoint.path || ''}</h2>
                    </div>
                    <button class="explain-btn text-sm text-indigo-600 hover:text-indigo-800 font-semibold flex items-center gap-1" data-endpoint-id="${endpoint.id}">Explain ✨</button>
                </div>
                <p class="text-slate-600 mb-6">${endpoint.description}</p>
                <div class="border-t border-slate-200 pt-4"><h4 class="font-semibold text-slate-700 mb-2">Authentication</h4><p class="text-sm text-slate-600">${endpoint.auth}</p></div>`;
            
            if (endpoint.parameters) {
                contentHtml += '<div class="border-t border-slate-200 pt-4 mt-4"><h4 class="font-semibold text-slate-700 mb-2">Query Parameters</h4><ul class="divide-y divide-slate-200">';
                endpoint.parameters.forEach(p => {
                    contentHtml += `<li class="py-2"><code class="bg-slate-200 p-1 rounded text-sm font-mono">${p.name}</code> <span class="text-xs text-slate-500">${p.type}</span><p class="text-sm text-slate-600 mt-1">${p.desc}</p></li>`;
                });
                contentHtml += '</ul></div>';
            }
            contentHtml += `</div>`;
            contentContainer.innerHTML += contentHtml;

            let codeHtml = `<div class="code-panel" id="code-${endpoint.id}">`;
            if (endpoint.request) {
                codeHtml += `<h3 class="text-lg font-semibold mb-2 text-slate-800">Request Body</h3><div class="relative"><button class="copy-btn absolute top-2 right-2 bg-slate-700 text-white text-xs px-2 py-1 rounded">Copy</button><pre class="bg-slate-800 text-white p-4 rounded-md text-sm overflow-x-auto"><code class="language-json">${JSON.stringify(endpoint.request, null, 2)}</code></pre></div>`;
            } else {
                 codeHtml += `<h3 class="text-lg font-semibold mb-2 text-slate-800">Request Body</h3><p class="text-sm text-slate-600">No request body required.</p>`;
            }
            if (endpoint.response) {
                 codeHtml += `<h3 class="text-lg font-semibold mt-6 mb-2 text-slate-800">Success Response (200 OK)</h3><div class="relative"><button class="copy-btn absolute top-2 right-2 bg-slate-700 text-white text-xs px-2 py-1 rounded">Copy</button><pre class="bg-slate-800 text-white p-4 rounded-md text-sm overflow-x-auto"><code class="language-json">${JSON.stringify(endpoint.response, null, 2)}</code></pre></div>`;
            }
            
            codeHtml += `
                <div class="mt-6 border-t border-slate-300 pt-4">
                     <h3 class="text-lg font-semibold mb-3 text-slate-800">Generate Code Snippet ✨</h3>
                     <div class="flex items-center gap-2 mb-3">
                        <select class="code-lang-select bg-white border border-slate-300 rounded-md text-sm px-2 py-1.5 w-full">
                            <option value="javascript">JavaScript (fetch)</option>
                            <option value="python">Python (requests)</option>
                            <option value="curl">cURL</option>
                        </select>
                        <button class="generate-code-btn bg-indigo-600 text-white font-semibold text-sm px-4 py-1.5 rounded-md hover:bg-indigo-700 flex-shrink-0" data-endpoint-id="${endpoint.id}">Generate</button>
                     </div>
                     <div class="generated-code-container min-h-[100px]"></div>
                </div>
            `;
            codeHtml += '</div>';
            codeContainer.innerHTML += codeHtml;
        });
        navHtml += `</ul></li>`;
    });

    navHtml += '</ul>';
    navContainer.innerHTML = navHtml;

    navContainer.addEventListener('click', (e) => {
        const link = e.target.closest('.nav-link');
        if (!link) return;
        e.preventDefault();
        const targetId = link.dataset.target;
        
        document.querySelectorAll('.nav-link').forEach(l => l.classList.remove('active'));
        document.querySelectorAll('.nav-group-title').forEach(t => t.classList.remove('active'));
        
        link.classList.add('active');
        const parentGroupTitle = link.closest('ul')?.previousElementSibling;
        if(parentGroupTitle && parentGroupTitle.classList.contains('nav-group-title')) {
            parentGroupTitle.classList.add('active');
        }

        document.querySelectorAll('.content-panel').forEach(p => p.classList.remove('active'));
        document.getElementById(`content-${targetId}`).classList.add('active');

        document.querySelectorAll('.code-panel').forEach(p => p.classList.remove('active'));
        document.getElementById(`code-${targetId}`).classList.add('active');
    });

    document.querySelector('.nav-link[data-target="introduction"]').click();

    document.body.addEventListener('click', (e) => {
        const copyBtn = e.target.closest('.copy-btn');
        const explainBtn = e.target.closest('.explain-btn');
        const generateBtn = e.target.closest('.generate-code-btn');
        
        if (copyBtn) {
            const pre = copyBtn.nextElementSibling;
            const code = pre.querySelector('code');
            if (code) {
                const textarea = document.createElement('textarea');
                textarea.value = code.innerText;
                textarea.style.position = 'absolute';
                textarea.style.left = '-9999px';
                document.body.appendChild(textarea);
                textarea.select();
                try {
                    document.execCommand('copy');
                    copyBtn.textContent = 'Copied!';
                } catch (err) {
                    copyBtn.textContent = 'Error';
                    console.error('Failed to copy text: ', err);
                }
                document.body.removeChild(textarea);
                setTimeout(() => { copyBtn.textContent = 'Copy'; }, 2000);
            }
        }

        if(explainBtn) {
            const endpointId = explainBtn.dataset.endpointId;
            const endpoint = apiData.services.flatMap(s => s.endpoints).find(e => e.id === endpointId);
            modal.classList.remove('hidden');
            modal.classList.add('flex');
            modalContent.innerHTML = '<div class="flex justify-center items-center p-8"><div class="loader"></div></div>';
            const prompt = `Explain the following API endpoint in simple terms for a new developer. What does it do and when would you use it? Keep it concise and use markdown for formatting (like **bold** text). Endpoint details: ${JSON.stringify(endpoint)}`;
            callGemini(prompt).then(explanation => {
                modalContent.innerHTML = parseSimpleMarkdown(explanation);
            });
        }

        if(generateBtn) {
            const container = generateBtn.closest('div').nextElementSibling;
            const langSelect = generateBtn.previousElementSibling;
            const language = langSelect.value;
            const endpointId = generateBtn.dataset.endpointId;
            const endpoint = apiData.services.flatMap(s => s.endpoints).find(e => e.id === endpointId);
            
            container.innerHTML = '<div class="flex justify-center items-center p-8"><div class="loader"></div></div>';
            
            const prompt = `Generate a code snippet in ${language} for the following API endpoint. The snippet should make a ${endpoint.method} request to a base URL + "${endpoint.path}". If a request body is needed, use this structure as an example: ${JSON.stringify(endpoint.request)}. Include comments explaining the code. Respond only with the code block.`;
            callGemini(prompt).then(code => {
                let cleanedCode = code.replace(/^```[a-z]*\n?/, '').replace(/```$/, '');
                container.innerHTML = `<div class="relative mt-2"><button class="copy-btn absolute top-2 right-2 bg-slate-700 text-white text-xs px-2 py-1 rounded">Copy</button><pre class="bg-slate-800 text-white p-4 rounded-md text-sm overflow-x-auto"><code>${cleanedCode}</code></pre></div>`;
            });
        }
    });

    // Modal close logic
    closeModalBtn.addEventListener('click', () => {
        modal.classList.add('hidden');
        modal.classList.remove('flex');
    });
    modal.addEventListener('click', (e) => {
        if (e.target === modal) {
            modal.classList.add('hidden');
            modal.classList.remove('flex');
        }
    });

    // AI Search logic
    let searchTimeout;
    searchInput.addEventListener('input', () => {
        clearTimeout(searchTimeout);
        searchTimeout = setTimeout(() => {
            const query = searchInput.value;
            if (query.length < 3) return;
            searchLoader.style.display = 'flex';
            const prompt = `Given the following list of API endpoints: "${endpointListForAI}". Which single endpoint ID is the most relevant for the user query: "${query}"? Respond with ONLY the endpoint ID string and nothing else (e.g., 'auth-login').`;
            callGemini(prompt).then(responseText => {
                let resultId = responseText.trim();
                const idMatch = resultId.match(/[a-z]+(?:-[a-z]+)+/);
                if (idMatch) {
                    resultId = idMatch[0];
                }
                const targetLink = document.querySelector(`.nav-link[data-target="${resultId}"]`);
                if (targetLink) {
                    targetLink.click();
                    targetLink.scrollIntoView({ behavior: 'smooth', block: 'center' });
                    targetLink.classList.add('highlight');
                    setTimeout(() => targetLink.classList.remove('highlight'), 2000);
                }
                 searchLoader.style.display = 'none';
            });
        }, 1000);
    });
});
</script>

</body>
</html>



# LocalFinder
this is wed to find local business shop and any service in your area in just one click
