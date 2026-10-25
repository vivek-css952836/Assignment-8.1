<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Assignment 8.1 - Dashboard-Style Grid Layout</title>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Chart.js for Interactive Charts -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <style>
        /* ==========================================================================
           ASSIGNMENT 8.1 - CSS GRID DASHBOARD STYLESHEET
           ========================================================================== */

        /* 1. CSS Variables for Modern Theme Management */
        :root {
            --bg-body: #f4f6f9;
            --bg-card: #ffffff;
            --bg-header: #ffffff;
            --bg-sidebar: #1e293b;
            --text-main: #0f172a;
            --text-muted: #64748b;
            --text-sidebar: #94a3b8;
            --text-sidebar-active: #ffffff;
            --accent-primary: #3b82f6;
            --accent-hover: #2563eb;
            --accent-bg: #eff6ff;
            --border-color: #e2e8f0;
            --shadow-sm: 0 1px 3px rgba(0,0,0,0.05);
            --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -1px rgba(0, 0, 0, 0.03);
            --success-color: #10b981;
            --danger-color: #ef4444;
            --radius-lg: 12px;
            --radius-md: 8px;
            --transition: all 0.25s ease-in-out;
        }

        /* Dark Theme Overrides */
        [data-theme="dark"] {
            --bg-body: #0f172a;
            --bg-card: #1e293b;
            --bg-header: #1e293b;
            --bg-sidebar: #0f172a;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --text-sidebar: #64748b;
            --accent-bg: rgba(59, 130, 246, 0.15);
            --border-color: #334155;
            --shadow-sm: 0 1px 3px rgba(0,0,0,0.3);
            --shadow-md: 0 4px 6px -1px rgba(0, 0, 0, 0.3);
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Inter', system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
        }

        body {
            background-color: var(--bg-body);
            color: var(--text-main);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            transition: background-color 0.3s ease, color 0.3s ease;
        }

        /* --------------------------------------------------------------------------
           2. OUTER GRID LAYOUT (Core Requirement)
           -------------------------------------------------------------------------- */
        .dashboard-container {
            display: grid;
            height: 100vh;
            width: 100vw;
            grid-template-columns: 260px 1fr; /* Column 1: Sidebar, Column 2: Main Content */
            grid-template-rows: 70px 1fr;    /* Row 1: Header, Row 2: Content Body */
            grid-template-areas: 
                "sidebar header"
                "sidebar main";
            gap: 0px; /* Outer layout seamlessly connected, internal items use grid gaps */
            overflow: hidden;
        }

        /* Responsive Grid Breakdown for Mobile & Tablet */
        @media (max-width: 900px) {
            .dashboard-container {
                grid-template-columns: 1fr;
                grid-template-rows: 70px auto 1fr;
                grid-template-areas: 
                    "header"
                    "sidebar"
                    "main";
                height: auto;
                min-height: 100vh;
                overflow: auto;
            }
        }

        /* --------------------------------------------------------------------------
           3. HEADER AREA (grid-area: header)
           -------------------------------------------------------------------------- */
        .dashboard-header {
            grid-area: header;
            background-color: var(--bg-header);
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 0 24px;
            z-index: 10;
        }

        .header-search {
            display: flex;
            align-items: center;
            background-color: var(--bg-body);
            padding: 8px 16px;
            border-radius: var(--radius-md);
            border: 1px solid var(--border-color);
            width: 300px;
        }

        .header-search i {
            color: var(--text-muted);
            margin-right: 10px;
        }

        .header-search input {
            border: none;
            outline: none;
            background: transparent;
            color: var(--text-main);
            width: 100%;
            font-size: 0.9rem;
        }

        .header-actions {
            display: flex;
            align-items: center;
            gap: 16px;
        }

        .icon-btn {
            background: var(--bg-body);
            border: 1px solid var(--border-color);
            color: var(--text-main);
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            transition: var(--transition);
            position: relative;
        }

        .icon-btn:hover {
            background: var(--accent-bg);
            color: var(--accent-primary);
        }

        .user-profile {
            display: flex;
            align-items: center;
            gap: 12px;
            padding-left: 12px;
            border-left: 1px solid var(--border-color);
        }

        .avatar {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: var(--accent-primary);
            color: white;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
        }

        .user-info {
            display: flex;
            flex-direction: column;
        }

        .user-info .name {
            font-weight: 600;
            font-size: 0.9rem;
        }

        .user-info .role {
            font-size: 0.75rem;
            color: var(--text-muted);
        }

        /* --------------------------------------------------------------------------
           4. SIDEBAR AREA (grid-area: sidebar)
           -------------------------------------------------------------------------- */
        .dashboard-sidebar {
            grid-area: sidebar;
            background-color: var(--bg-sidebar);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 24px 16px;
            z-index: 20;
        }

        .brand {
            display: flex;
            align-items: center;
            gap: 12px;
            color: white;
            font-size: 1.25rem;
            font-weight: 700;
            padding: 0 12px 24px 12px;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }

        .brand i {
            color: var(--accent-primary);
        }

        .nav-list {
            list-style: none;
            margin-top: 24px;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }

        .nav-item a {
            display: flex;
            align-items: center;
            gap: 14px;
            padding: 12px 16px;
            color: var(--text-sidebar);
            text-decoration: none;
            border-radius: var(--radius-md);
            font-size: 0.95rem;
            font-weight: 500;
            transition: var(--transition);
        }

        .nav-item a:hover, .nav-item.active a {
            background-color: rgba(255,255,255,0.08);
            color: var(--text-sidebar-active);
        }

        .nav-item.active a {
            background-color: var(--accent-primary);
        }

        .submission-code-btn {
            background: rgba(255, 255, 255, 0.05);
            border: 1px dashed var(--accent-primary);
            color: #60a5fa;
            padding: 12px;
            border-radius: var(--radius-md);
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            font-weight: 600;
            font-size: 0.85rem;
            transition: var(--transition);
        }

        .submission-code-btn:hover {
            background: var(--accent-primary);
            color: white;
        }

        /* Mobile Sidebar adjustments */
        @media (max-width: 900px) {
            .dashboard-sidebar {
                padding: 12px;
            }
            .nav-list {
                flex-direction: row;
                overflow-x: auto;
                margin-top: 0;
            }
            .brand { display: none; }
        }

        /* --------------------------------------------------------------------------
           5. MAIN CONTENT AREA (grid-area: main)
           -------------------------------------------------------------------------- */
        .dashboard-main {
            grid-area: main;
            padding: 28px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
            gap: 24px;
        }

        .page-title-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 16px;
        }

        .page-title h1 {
            font-size: 1.5rem;
            font-weight: 700;
        }

        .page-title p {
            font-size: 0.875rem;
            color: var(--text-muted);
            margin-top: 2px;
        }

        .action-bar {
            display: flex;
            gap: 12px;
        }

        .btn {
            padding: 10px 18px;
            border-radius: var(--radius-md);
            border: none;
            font-weight: 600;
            font-size: 0.875rem;
            cursor: pointer;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            transition: var(--transition);
        }

        .btn-primary {
            background-color: var(--accent-primary);
            color: white;
        }

        .btn-primary:hover {
            background-color: var(--accent-hover);
        }

        .btn-secondary {
            background-color: var(--bg-card);
            color: var(--text-main);
            border: 1px solid var(--border-color);
        }

        .btn-secondary:hover {
            background-color: var(--bg-body);
        }

        /* --------------------------------------------------------------------------
           6. SUB-GRID LAYOUT: STATISTIC CARDS (Requirement 3)
           -------------------------------------------------------------------------- */
        .stats-grid {
            display: grid;
            /* Explicit responsive multi-column layout for cards using minmax */
            grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
            gap: 20px;
        }

        .stat-card {
            background-color: var(--bg-card);
            border: 1px solid var(--border-color);
            border-radius: var(--radius-lg);
            padding: 20px;
            box-shadow: var(--shadow-sm);
            transition: var(--transition);
            display: flex;
            flex-direction: column;
            justify-content: space-between;
        }

        .stat-card:hover {
            transform: translateY(-3px);
            box-shadow: var(--shadow-md);
        }

        .stat-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            color: var(--text-muted);
            font-size: 0.875rem;
            font-weight: 500;
        }

        .stat-icon {
            width: 42px;
            height: 42px;
            border-radius: var(--radius-md);
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.2rem;
        }

        .stat-icon.blue { background: rgba(59, 130, 246, 0.1); color: #3b82f6; }
        .stat-icon.green { background: rgba(16, 185, 129, 0.1); color: #10b981; }
        .stat-icon.purple { background: rgba(139, 92, 246, 0.1); color: #8b5cf6; }
        .stat-icon.orange { background: rgba(245, 158, 11, 0.1); color: #f59e0b; }

        .stat-body {
            margin-top: 16px;
        }

        .stat-value {
            font-size: 1.8rem;
            font-weight: 700;
        }

        .stat-footer {
            display: flex;
            align-items: center;
            gap: 6px;
            margin-top: 8px;
            font-size: 0.8rem;
        }

        .badge {
            display: inline-flex;
            align-items: center;
            gap: 4px;
            padding: 2px 6px;
            border-radius: 4px;
            font-weight: 600;
        }

        .badge.positive { background: rgba(16, 185, 129, 0.15); color: var(--success-color); }
        .badge.negative { background: rgba(239, 68, 68, 0.15); color: var(--danger-color); }

        /* --------------------------------------------------------------------------
           7. VISUAL WIDGETS GRID LAYOUT
           -------------------------------------------------------------------------- */
        .analytics-widgets-grid {
            display: grid;
            grid-template-columns: 2fr 1fr;
            gap: 20px;
        }

        @media (max-width: 1024px) {
            .analytics-widgets-grid {
                grid-template-columns: 1fr;
            }
        }

        .widget-card {
            background-color: var(--bg-card);
            border: 1px solid var(--border-color);
            border-radius: var(--radius-lg);
            padding: 20px;
            box-shadow: var(--shadow-sm);
        }

        .widget-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        .widget-title {
            font-size: 1.1rem;
            font-weight: 600;
        }

        /* Progress Bars */
        .progress-list {
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        .progress-item-info {
            display: flex;
            justify-content: space-between;
            font-size: 0.875rem;
            margin-bottom: 6px;
        }

        .progress-bar-bg {
            height: 8px;
            background-color: var(--bg-body);
            border-radius: 4px;
            overflow: hidden;
        }

        .progress-bar-fill {
            height: 100%;
            border-radius: 4px;
            background-color: var(--accent-primary);
        }

        /* Recent Activity List */
        .activity-list {
            display: flex;
            flex-direction: column;
            gap: 16px;
        }

        .activity-item {
            display: flex;
            gap: 12px;
            align-items: flex-start;
            font-size: 0.875rem;
        }

        .activity-avatar {
            width: 32px;
            height: 32px;
            border-radius: 50%;
            background: var(--accent-bg);
            color: var(--accent-primary);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 0.75rem;
            flex-shrink: 0;
        }

        .activity-time {
            font-size: 0.75rem;
            color: var(--text-muted);
            margin-top: 2px;
        }

        /* --------------------------------------------------------------------------
           8. CODE SUBMISSION MODAL / SOURCE VIEW
           -------------------------------------------------------------------------- */
        .modal-overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100vw;
            height: 100vh;
            background: rgba(0, 0, 0, 0.6);
            display: none;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            padding: 20px;
        }

        .modal-overlay.active {
            display: flex;
        }

        .modal-box {
            background: var(--bg-card);
            border-radius: var(--radius-lg);
            width: 100%;
            max-width: 850px;
            max-height: 85vh;
            display: flex;
            flex-direction: column;
            overflow: hidden;
            border: 1px solid var(--border-color);
            box-shadow: var(--shadow-md);
        }

        .modal-header {
            padding: 16px 20px;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .modal-tabs {
            display: flex;
            gap: 10px;
            background: var(--bg-body);
            padding: 4px;
            border-radius: var(--radius-md);
        }

        .tab-btn {
            padding: 6px 14px;
            border: none;
            background: transparent;
            color: var(--text-muted);
            cursor: pointer;
            border-radius: 4px;
            font-weight: 600;
            font-size: 0.85rem;
        }

        .tab-btn.active {
            background: var(--bg-card);
            color: var(--text-main);
            box-shadow: var(--shadow-sm);
        }

        .modal-body {
            padding: 16px;
            overflow-y: auto;
            background: #1e1e1e;
            color: #d4d4d4;
            font-family: monospace;
            font-size: 0.85rem;
            line-height: 1.5;
        }

        .modal-footer {
            padding: 12px 20px;
            border-top: 1px solid var(--border-color);
            display: flex;
            justify-content: flex-end;
            gap: 10px;
        }
    </style>
</head>
<body>

    <!-- 1. MAIN OUTER DASHBOARD GRID CONTAINER -->
    <div class="dashboard-container">
        
        <!-- HEADER (Grid Area: header) -->
        <header class="dashboard-header">
            <div class="header-search">
                <i class="fa-solid fa-magnifying-glass"></i>
                <input type="text" placeholder="Search analytics, metrics, reports..." id="searchInput">
            </div>

            <div class="header-actions">
                <!-- Theme Switcher Toggle -->
                <button class="icon-btn" id="themeToggleBtn" title="Toggle Light/Dark Theme">
                    <i class="fa-solid fa-moon"></i>
                </button>
                <button class="icon-btn" title="Notifications">
                    <i class="fa-solid fa-bell"></i>
                </button>
                <div class="user-profile">
                    <div class="avatar">JD</div>
                    <div class="user-info">
                        <span class="name">John Doe</span>
                        <span class="role">Admin User</span>
                    </div>
                </div>
            </div>
        </header>

        <!-- SIDEBAR (Grid Area: sidebar) -->
        <aside class="dashboard-sidebar">
            <div>
                <div class="brand">
                    <i class="fa-solid fa-chart-line"></i>
                    <span>GridDash</span>
                </div>
                <ul class="nav-list">
                    <li class="nav-item active"><a href="#"><i class="fa-solid fa-house"></i> <span>Overview</span></a></li>
                    <li class="nav-item"><a href="#"><i class="fa-solid fa-chart-pie"></i> <span>Analytics</span></a></li>
                    <li class="nav-item"><a href="#"><i class="fa-solid fa-folder"></i> <span>Projects</span></a></li>
                    <li class="nav-item"><a href="#"><i class="fa-solid fa-file-invoice"></i> <span>Reports</span></a></li>
                    <li class="nav-item"><a href="#"><i class="fa-solid fa-gear"></i> <span>Settings</span></a></li>
                </ul>
            </div>

            <button class="submission-code-btn" id="openSourceBtn">
                <i class="fa-solid fa-code"></i> View Assignment Code
            </button>
        </aside>

        <!-- MAIN CONTENT AREA (Grid Area: main) -->
        <main class="dashboard-main">
            <!-- Header section inside main area -->
            <div class="page-title-bar">
                <div class="page-title">
                    <h1>Analytics Dashboard</h1>
                    <p>Overview of system metrics, active traffic, and revenue stats.</p>
                </div>
                <div class="action-bar">
                    <button class="btn btn-secondary" onclick="triggerNotification('Filter dialog opened')">
                        <i class="fa-solid fa-filter"></i> Filter Data
                    </button>
                    <button class="btn btn-primary" onclick="triggerNotification('Report exported successfully!')">
                        <i class="fa-solid fa-download"></i> Export Report
                    </button>
                </div>
            </div>

            <!-- STATISTIC CARDS GRID (Requirement 3 & Sub-Grid arrangement) -->
            <section class="stats-grid">
                <!-- Card 1: Total Users -->
                <div class="stat-card">
                    <div class="stat-header">
                        <span>Total Users</span>
                        <div class="stat-icon blue"><i class="fa-solid fa-users"></i></div>
                    </div>
                    <div class="stat-body">
                        <div class="stat-value" id="val-users">24,512</div>
                        <div class="stat-footer">
                            <span class="badge positive"><i class="fa-solid fa-arrow-up"></i> +12.5%</span>
                            <span style="color: var(--text-muted)">vs last month</span>
                        </div>
                    </div>
                </div>

                <!-- Card 2: Revenue -->
                <div class="stat-card">
                    <div class="stat-header">
                        <span>Revenue</span>
                        <div class="stat-icon green"><i class="fa-solid fa-dollar-sign"></i></div>
                    </div>
                    <div class="stat-body">
                        <div class="stat-value" id="val-revenue">$45,210</div>
                        <div class="stat-footer">
                            <span class="badge positive"><i class="fa-solid fa-arrow-up"></i> +8.2%</span>
                            <span style="color: var(--text-muted)">vs last month</span>
                        </div>
                    </div>
                </div>

                <!-- Card 3: Active Subscriptions -->
                <div class="stat-card">
                    <div class="stat-header">
                        <span>Active Subscriptions</span>
                        <div class="stat-icon purple"><i class="fa-solid fa-credit-card"></i></div>
                    </div>
                    <div class="stat-body">
                        <div class="stat-value" id="val-subs">1,420</div>
                        <div class="stat-footer">
                            <span class="badge positive"><i class="fa-solid fa-arrow-up"></i> +5.4%</span>
                            <span style="color: var(--text-muted)">vs last month</span>
                        </div>
                    </div>
                </div>

                <!-- Card 4: Bounce Rate -->
                <div class="stat-card">
                    <div class="stat-header">
                        <span>Bounce Rate</span>
                        <div class="stat-icon orange"><i class="fa-solid fa-chart-line-down"></i></div>
                    </div>
                    <div class="stat-body">
                        <div class="stat-value" id="val-bounce">24.3%</div>
                        <div class="stat-footer">
                            <span class="badge negative"><i class="fa-solid fa-arrow-down"></i> -3.1%</span>
                            <span style="color: var(--text-muted)">vs last month</span>
                        </div>
                    </div>
                </div>
            </section>

            <!-- ADDITIONAL ANALYTICS & CHARTS GRID -->
            <section class="analytics-widgets-grid">
                <!-- Revenue Overview Chart Card -->
                <div class="widget-card">
                    <div class="widget-header">
                        <h2 class="widget-title">Revenue & Traffic Trend</h2>
                        <button class="btn btn-secondary" style="padding: 4px 10px; font-size: 0.75rem;" onclick="refreshChartData()">
                            <i class="fa-solid fa-arrows-rotate"></i> Refresh
                        </button>
                    </div>
                    <div style="position: relative; height:280px; width:100%">
                        <canvas id="revenueChart"></canvas>
                    </div>
                </div>

                <!-- Traffic Sources / Progress Card -->
                <div class="widget-card">
                    <div class="widget-header">
                        <h2 class="widget-title">Traffic Sources</h2>
                    </div>
                    <div class="progress-list">
                        <div>
                            <div class="progress-item-info">
                                <span>Direct Search</span>
                                <strong>45%</strong>
                            </div>
                            <div class="progress-bar-bg">
                                <div class="progress-bar-fill" style="width: 45%;"></div>
                            </div>
                        </div>

                        <div>
                            <div class="progress-item-info">
                                <span>Social Media</span>
                                <strong>28%</strong>
                            </div>
                            <div class="progress-bar-bg">
                                <div class="progress-bar-fill" style="width: 28%; background-color: #8b5cf6;"></div>
                            </div>
                        </div>

                        <div>
                            <div class="progress-item-info">
                                <span>Referral Links</span>
                                <strong>18%</strong>
                            </div>
                            <div class="progress-bar-bg">
                                <div class="progress-bar-fill" style="width: 18%; background-color: #10b981;"></div>
                            </div>
                        </div>

                        <div>
                            <div class="progress-item-info">
                                <span>Email Campaigns</span>
                                <strong>9%</strong>
                            </div>
                            <div class="progress-bar-bg">
                                <div class="progress-bar-fill" style="width: 9%; background-color: #f59e0b;"></div>
                            </div>
                        </div>
                    </div>
                </div>
            </section>

            <!-- RECENT ACTIVITY SECTION -->
            <section class="widget-card">
                <div class="widget-header">
                    <h2 class="widget-title">Recent System Activity</h2>
                </div>
                <div class="activity-list">
                    <div class="activity-item">
                        <div class="activity-avatar">JS</div>
                        <div>
                            <div><strong>Jane Smith</strong> created a new subscription.</div>
                            <div class="activity-time">5 minutes ago</div>
                        </div>
                    </div>
                    <div class="activity-item">
                        <div class="activity-avatar" style="background: rgba(16,185,129,0.15); color: #10b981;">SR</div>
                        <div>
                            <div><strong>System Alert:</strong> Monthly data export completed automatically.</div>
                            <div class="activity-time">1 hour ago</div>
                        </div>
                    </div>
                    <div class="activity-item">
                        <div class="activity-avatar" style="background: rgba(245,158,11,0.15); color: #f59e0b;">MW</div>
                        <div>
                            <div><strong>Mark Wilson</strong> updated security permissions in Settings.</div>
                            <div class="activity-time">3 hours ago</div>
                        </div>
                    </div>
                </div>
            </section>

        </main>
    </div>

    <!-- CODE VIEW / ASSIGNMENT SUBMISSION MODAL -->
    <div class="modal-overlay" id="codeModal">
        <div class="modal-box">
            <div class="modal-header">
                <div class="modal-tabs">
                    <button class="tab-btn active" onclick="showTab('cssTab', this)">CSS Grid Styles</button>
                    <button class="tab-btn" onclick="showTab('htmlTab', this)">HTML Structure</button>
                </div>
                <button class="icon-btn" onclick="closeModal()"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div class="modal-body">
                <pre id="cssTab"><code id="cssCodeContainer">
/* Outer CSS Grid Container Layout Definition */
.dashboard-container {
    display: grid;
    height: 100vh;
    width: 100vw;
    grid-template-columns: 260px 1fr; /* Column 1: Sidebar, Column 2: Main Content */
    grid-template-rows: 70px 1fr;    /* Row 1: Header, Row 2: Content Area */
    grid-template-areas: 
        "sidebar header"
        "sidebar main";
    gap: 0px;
}

/* Responsive Grid Adjustments */
@media (max-width: 900px) {
    .dashboard-container {
        grid-template-columns: 1fr;
        grid-template-rows: 70px auto 1fr;
        grid-template-areas: 
            "header"
            "sidebar"
            "main";
    }
}

/* Header Grid Placement */
.dashboard-header {
    grid-area: header;
}

/* Sidebar Grid Placement */
.dashboard-sidebar {
    grid-area: sidebar;
}

/* Main Dashboard Area Grid Placement */
.dashboard-main {
    grid-area: main;
}

/* Statistic Cards Sub-Grid Configuration */
.stats-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
    gap: 20px;
}
                </code></pre>

                <pre id="htmlTab" style="display:none;"><code id="htmlCodeContainer">
&lt;div class="dashboard-container"&gt;
    &lt;!-- Header Section --&gt;
    &lt;header class="dashboard-header"&gt; ... &lt;/header&gt;

    &lt;!-- Sidebar Section --&gt;
    &lt;aside class="dashboard-sidebar"&gt; ... &lt;/aside&gt;

    &lt;!-- Main Content Section --&gt;
    &lt;main class="dashboard-main"&gt;
        &lt;section class="stats-grid"&gt;
            &lt;div class="stat-card"&gt;Total Users: 24,512&lt;/div&gt;
            &lt;div class="stat-card"&gt;Revenue: $45,210&lt;/div&gt;
            &lt;div class="stat-card"&gt;Active Subscriptions: 1,420&lt;/div&gt;
            &lt;div class="stat-card"&gt;Bounce Rate: 24.3%&lt;/div&gt;
        &lt;/section&gt;
    &lt;/main&gt;
&lt;/div&gt;
                </code></pre>
            </div>
            <div class="modal-footer">
                <button class="btn btn-secondary" onclick="closeModal()">Close</button>
                <button class="btn btn-primary" onclick="copyActiveCode()"><i class="fa-solid fa-copy"></i> Copy Code</button>
            </div>
        </div>
    </div>

    <!-- JAVASCRIPT LOGIC -->
    <script>
        // 1. Theme Toggle (Light/Dark Mode)
        const themeToggleBtn = document.getElementById('themeToggleBtn');
        let currentTheme = 'light';

        themeToggleBtn.addEventListener('click', () => {
            if (currentTheme === 'light') {
                document.documentElement.setAttribute('data-theme', 'dark');
                themeToggleBtn.innerHTML = '<i class="fa-solid fa-sun"></i>';
                currentTheme = 'dark';
            } else {
                document.documentElement.removeAttribute('data-theme');
                themeToggleBtn.innerHTML = '<i class="fa-solid fa-moon"></i>';
                currentTheme = 'light';
            }
            updateChartTheme();
        });

        // 2. Chart.js Setup for Data Visualization
        let myChart;
        function initChart() {
            const ctx = document.getElementById('revenueChart').getContext('2d');
            
            myChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul'],
                    datasets: [{
                        label: 'Revenue ($)',
                        data: [12000, 19000, 15000, 28000, 22000, 35000, 45210],
                        borderColor: '#3b82f6',
                        backgroundColor: 'rgba(59, 130, 246, 0.1)',
                        fill: true,
                        tension: 0.4,
                        borderWidth: 2
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    plugins: {
                        legend: { display: false }
                    },
                    scales: {
                        x: { grid: { display: false } },
                        y: { grid: { color: 'rgba(200,200,200,0.15)' } }
                    }
                }
            });
        }

        function updateChartTheme() {
            if (!myChart) return;
            const isDark = currentTheme === 'dark';
            myChart.options.scales.x.ticks.color = isDark ? '#94a3b8' : '#64748b';
            myChart.options.scales.y.ticks.color = isDark ? '#94a3b8' : '#64748b';
            myChart.update();
        }

        function refreshChartData() {
            if (!myChart) return;
            const randomData = Array.from({length: 7}, () => Math.floor(Math.random() * 30000) + 15000);
            myChart.data.datasets[0].data = randomData;
            myChart.update();
            triggerNotification('Chart metrics updated!');
        }

        // 3. Simple Search Filter Mock Interaction
        document.getElementById('searchInput').addEventListener('input', function(e) {
            const query = e.target.value.toLowerCase();
            const cards = document.querySelectorAll('.stat-card');
            cards.forEach(card => {
                const text = card.textContent.toLowerCase();
                card.style.opacity = text.includes(query) ? '1' : '0.3';
            });
        });

        // 4. Modal Code Viewer Functionality
        const codeModal = document.getElementById('codeModal');
        document.getElementById('openSourceBtn').addEventListener('click', () => {
            codeModal.classList.add('active');
        });

        function closeModal() {
            codeModal.classList.remove('active');
        }

        function showTab(tabId, btn) {
            document.getElementById('cssTab').style.display = 'none';
            document.getElementById('htmlTab').style.display = 'none';
            document.querySelectorAll('.tab-btn').forEach(b => b.classList.remove('active'));

            document.getElementById(tabId).style.display = 'block';
            btn.classList.add('active');
        }

        function copyActiveCode() {
            const activeTab = document.getElementById('cssTab').style.display !== 'none' ? 'cssCodeContainer' : 'htmlCodeContainer';
            const codeText = document.getElementById(activeTab).innerText;
            
            // Clipboard fallback as specified
            const tempInput = document.createElement('textarea');
            tempInput.value = codeText;
            document.body.appendChild(tempInput);
            tempInput.select();
            document.execCommand('copy');
            document.body.removeChild(tempInput);

            triggerNotification('Source code copied to clipboard!');
            closeModal();
        }

        // Helper Notification Banner
        function triggerNotification(msg) {
            const toast = document.createElement('div');
            toast.style.position = 'fixed';
            toast.style.bottom = '20px';
            toast.style.right = '20px';
            toast.style.background = '#0f172a';
            toast.style.color = '#fff';
            toast.style.padding = '12px 20px';
            toast.style.borderRadius = '8px';
            toast.style.boxShadow = '0 4px 12px rgba(0,0,0,0.15)';
            toast.style.zIndex = '2000';
            toast.style.fontSize = '0.875rem';
            toast.innerHTML = `<i class="fa-solid fa-circle-check" style="color: #10b981; margin-right: 8px;"></i> ${msg}`;
            
            document.body.appendChild(toast);
            setTimeout(() => {
                toast.style.opacity = '0';
                toast.style.transition = 'opacity 0.5s ease';
                setTimeout(() => toast.remove(), 500);
            }, 2500);
        }

        // Initialize on page load
        window.onload = function() {
            initChart();
        };
    </script>
</body>
</html>