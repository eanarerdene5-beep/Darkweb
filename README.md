<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>BlackCult - Private Group Voting</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts for Gothic/Futuristic minimal look -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@400;700;900&family=Inter:wght@300;400;600;800&display=swap" rel="stylesheet">
    <!-- Lucide Icons -->
    <script src="https://unpkg.com/lucide@latest"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        cult: {
                            black: '#0a0a0c',
                            darker: '#050506',
                            card: '#121216',
                            border: '#262630',
                            crimson: '#dc2626',
                            neon: '#ff1e43',
                            glow: 'rgba(255, 30, 67, 0.25)',
                            text: '#e2e8f0',
                            muted: '#94a3b8'
                        }
                    },
                    fontFamily: {
                        cinzel: ['Cinzel', 'serif'],
                        sans: ['Inter', 'sans-serif'],
                    },
                    boxShadow: {
                        'neon-glow': '0 0 20px rgba(255, 30, 67, 0.35)',
                        'neon-subtle': '0 0 10px rgba(255, 30, 67, 0.15)',
                    }
                }
            }
        }
    </script>
    <style>
        body {
            background-color: #050506;
            color: #e2e8f0;
            font-family: 'Inter', sans-serif;
            background-image: 
                radial-gradient(circle at 50% 0%, rgba(220, 38, 38, 0.15), transparent 50%),
                radial-gradient(circle at 80% 80%, rgba(255, 30, 67, 0.05), transparent 40%);
            min-height: 100vh;
        }

        .cult-title {
            font-family: 'Cinzel', serif;
            letter-spacing: 0.25em;
        }

        .crimson-gradient-text {
            background: linear-gradient(135deg, #ffffff 0%, #ff1e43 100%);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .glass-panel {
            background: rgba(18, 18, 22, 0.75);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 30, 67, 0.15);
        }

        .glass-panel:hover {
            border-color: rgba(255, 30, 67, 0.35);
        }

        /* Custom scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #050506;
        }
        ::-webkit-scrollbar-thumb {
            background: #262630;
            border-radius: 3px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #ff1e43;
        }
    </style>
</head>
<body class="flex flex-col min-h-screen text-cult-text antialiased selection:bg-cult-neon selection:text-white">

    <!-- Top Navigation Header -->
    <header class="border-b border-cult-border bg-cult-darker/80 backdrop-blur-md sticky top-0 z-50">
        <div class="max-w-6xl mx-auto px-4 py-4 flex items-center justify-between">
            <div class="flex items-center space-x-3 cursor-pointer" onclick="switchView('landing')">
                <div class="w-9 h-9 rounded-full bg-cult-black border border-cult-neon/50 flex items-center justify-center shadow-neon-subtle">
                    <i data-lucide="shield" class="w-5 h-5 text-cult-neon"></i>
                </div>
                <span class="cult-title text-xl font-bold tracking-widest crimson-gradient-text">BLACKCULT</span>
            </div>
            
            <div id="nav-session-info" class="hidden items-center space-x-4">
                <span class="text-xs uppercase tracking-widest text-cult-muted hidden sm:inline">Active Circle:</span>
                <span id="nav-group-name" class="font-cinzel font-semibold text-cult-neon bg-cult-neon/10 px-3 py-1 rounded border border-cult-neon/20">--</span>
                <button onclick="leaveGroup()" class="text-xs text-cult-muted hover:text-cult-neon transition flex items-center space-x-1">
                    <i data-lucide="log-out" class="w-3.5 h-3.5"></i>
                    <span>Leave</span>
                </button>
            </div>
        </div>
    </header>

    <!-- Main Container -->
    <main class="flex-grow max-w-6xl w-full mx-auto p-4 sm:p-6 lg:p-8 flex flex-col justify-center">

        <!-- Notification Banner -->
        <div id="toast-container" class="fixed top-20 right-4 z-50 flex flex-col space-y-2 pointer-events-none"></div>

        <!-- VIEW 1: LANDING & SESSION CREATION / JOINING -->
        <section id="view-landing" class="space-y-12 my-auto py-8">
            <div class="text-center max-w-2xl mx-auto space-y-4">
                <div class="inline-flex items-center space-x-2 px-3 py-1 rounded-full border border-cult-neon/30 bg-cult-neon/10 text-cult-neon text-xs tracking-widest uppercase mb-2">
                    <i data-lucide="lock" class="w-3.5 h-3.5"></i>
                    <span>Private Anonymous Consensus</span>
                </div>
                <h1 class="cult-title text-4xl sm:text-5xl font-extrabold tracking-wider text-white">
                    ONE CIRCLE. ONE CHOICE.
                </h1>
                <p class="text-cult-muted text-sm sm:text-base leading-relaxed">
                    Create an encrypted-style private room. Invite candidates into the altar. Every member casts <strong class="text-white font-medium">exactly 1 vote for 1 guy</strong>. Results are calculated in real time.
                </p>
            </div>

            <div class="grid md:grid-cols-2 gap-6 max-w-4xl mx-auto">
                <!-- Create New Private Group Card -->
                <div class="glass-panel p-6 sm:p-8 rounded-xl shadow-2xl flex flex-col justify-between transition-all duration-300">
                    <div class="space-y-4">
                        <div class="w-12 h-12 rounded-lg bg-cult-neon/10 border border-cult-neon/30 flex items-center justify-center text-cult-neon">
                            <i data-lucide="plus-circle" class="w-6 h-6"></i>
                        </div>
                        <h2 class="text-xl font-bold font-cinzel text-white tracking-wide">Initiate New Circle</h2>
                        <p class="text-xs text-cult-muted">Establish a private group room, define candidates or members, and distribute the join code.</p>
                        
                        <div class="space-y-3 pt-2">
                            <div>
                                <label class="block text-xs uppercase tracking-wider text-cult-muted mb-1">Circle Title</label>
                                <input type="text" id="create-group-name" placeholder="e.g. Council of Shadow, Master Mind" 
                                    class="w-full bg-cult-black border border-cult-border rounded-lg px-4 py-2.5 text-sm text-white focus:outline-none focus:border-cult-neon transition">
                            </div>
                            <div>
                                <label class="block text-xs uppercase tracking-wider text-cult-muted mb-1">Your Alias / Voter Identity</label>
                                <input type="text" id="create-user-name" placeholder="e.g. Initiate #01" 
                                    class="w-full bg-cult-black border border-cult-border rounded-lg px-4 py-2.5 text-sm text-white focus:outline-none focus:border-cult-neon transition">
                            </div>
                            <div>
                                <label class="block text-xs uppercase tracking-wider text-cult-muted mb-1">Initial Candidates / Members (Comma separated)</label>
                                <input type="text" id="create-candidates" placeholder="e.g. Alex, Viktor, Marcus, Damien" 
                                    class="w-full bg-cult-black border border-cult-border rounded-lg px-4 py-2.5 text-sm text-white focus:outline-none focus:border-cult-neon transition">
                            </div>
                        </div>
                    </div>

                    <button onclick="handleCreateGroup()" class="mt-6 w-full bg-cult-neon hover:bg-red-600 text-white font-semibold py-3 px-4 rounded-lg shadow-neon-glow transition flex items-center justify-center space-x-2 text-sm uppercase tracking-wider">
                        <span>Summon Private Room</span>
                        <i data-lucide="arrow-right" class="w-4 h-4"></i>
                    </button>
                </div>

                <!-- Join Existing Group Card -->
                <div class="glass-panel p-6 sm:p-8 rounded-xl shadow-2xl flex flex-col justify-between transition-all duration-300">
                    <div class="space-y-4">
                        <div class="w-12 h-12 rounded-lg bg-cult-neon/10 border border-cult-neon/30 flex items-center justify-center text-cult-neon">
                            <i data-lucide="key" class="w-6 h-6"></i>
                        </div>
                        <h2 class="text-xl font-bold font-cinzel text-white tracking-wide">Enter Private Circle</h2>
                        <p class="text-xs text-cult-muted">Have a Circle Access Code? Enter below to participate in the vote.</p>

                        <div class="space-y-3 pt-2">
                            <div>
                                <label class="block text-xs uppercase tracking-wider text-cult-muted mb-1">Access Passcode / Code</label>
                                <input type="text" id="join-group-code" placeholder="Enter room code (e.g. CULT-8921)" 
                                    class="w-full bg-cult-black border border-cult-border rounded-lg px-4 py-2.5 text-sm text-white uppercase focus:outline-none focus:border-cult-neon transition">
                            </div>
                            <div>
                                <label class="block text-xs uppercase tracking-wider text-cult-muted mb-1">Your Alias</label>
                                <input type="text" id="join-user-name" placeholder="e.g. Shadow Pilgrim" 
                                    class="w-full bg-cult-black border border-cult-border rounded-lg px-4 py-2.5 text-sm text-white focus:outline-none focus:border-cult-neon transition">
                            </div>
                        </div>
                    </div>

                    <div class="pt-6">
                        <button onclick="handleJoinGroup()" class="w-full bg-transparent border border-cult-neon text-cult-neon hover:bg-cult-neon hover:text-white font-semibold py-3 px-4 rounded-lg transition flex items-center justify-center space-x-2 text-sm uppercase tracking-wider">
                            <span>Join Circle</span>
                            <i data-lucide="log-in" class="w-4 h-4"></i>
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- VIEW 2: ACTIVE VOTING & GROUP DASHBOARD -->
        <section id="view-voting" class="hidden space-y-8">
            <!-- Header bar for group status -->
            <div class="glass-panel p-6 rounded-xl flex flex-col lg:flex-row items-start lg:items-center justify-between gap-4">
                <div>
                    <div class="flex items-center space-x-3">
                        <h1 id="group-display-title" class="text-2xl sm:text-3xl font-extrabold font-cinzel text-white">--</h1>
                        <span id="user-status-badge" class="px-2.5 py-0.5 rounded text-xs uppercase tracking-wider font-semibold bg-yellow-500/10 text-yellow-500 border border-yellow-500/30">
                            Vote Pending
                        </span>
                    </div>
                    <p class="text-xs text-cult-muted mt-1">Logged in as: <span id="voter-display-name" class="text-white font-medium">--</span></p>
                </div>

                <div class="flex flex-wrap items-center gap-3 w-full lg:w-auto">
                    <!-- Room Code Share Button -->
                    <button onclick="copyGroupCode()" class="flex-1 lg:flex-none bg-cult-black border border-cult-border hover:border-cult-neon/50 px-3.5 py-2 rounded-lg text-xs font-mono text-cult-text flex items-center justify-center space-x-2 transition">
                        <i data-lucide="share-2" class="w-3.5 h-3.5 text-cult-neon"></i>
                        <span>Code: <strong id="display-room-code" class="text-white">--</strong></span>
                    </button>

                    <!-- Add candidate modal trigger -->
                    <button onclick="openAddCandidateModal()" class="flex-1 lg:flex-none bg-cult-black border border-cult-border hover:border-cult-neon/50 px-3.5 py-2 rounded-lg text-xs text-cult-text flex items-center justify-center space-x-1.5 transition">
                        <i data-lucide="user-plus" class="w-3.5 h-3.5 text-cult-neon"></i>
                        <span>Add Member</span>
                    </button>

                    <!-- Reset Votes Button -->
                    <button onclick="resetGroupVotes()" class="bg-cult-black border border-cult-border hover:border-red-500/50 px-3.5 py-2 rounded-lg text-xs text-cult-muted hover:text-red-400 flex items-center justify-center space-x-1.5 transition">
                        <i data-lucide="rotate-ccw" class="w-3.5 h-3.5"></i>
                        <span>Reset Session</span>
                    </button>
                </div>
            </div>

            <!-- Grid Layout for Voting & Live Analytics -->
            <div class="grid lg:grid-cols-12 gap-8">
                
                <!-- Left Column: Voting Cards (8 cols) -->
                <div class="lg:col-span-7 space-y-4">
                    <div class="flex items-center justify-between">
                        <h2 class="text-lg font-cinzel font-bold text-white flex items-center space-x-2">
                            <i data-lucide="users" class="w-5 h-5 text-cult-neon"></i>
                            <span>Candidates / Nominees</span>
                        </h2>
                        <span class="text-xs text-cult-muted">Rules: Strictly 1 vote per guy</span>
                    </div>

                    <div id="candidates-container" class="space-y-3">
                        <!-- Candidate Cards dynamically populated -->
                    </div>
                </div>

                <!-- Right Column: Live Tallies & Standings (5 cols) -->
                <div class="lg:col-span-5 space-y-4">
                    <h2 class="text-lg font-cinzel font-bold text-white flex items-center space-x-2">
                        <i data-lucide="bar-chart-3" class="w-5 h-5 text-cult-neon"></i>
                        <span>Live Tally Standing</span>
                    </h2>

                    <div class="glass-panel p-5 rounded-xl space-y-4">
                        <div class="flex items-center justify-between text-xs text-cult-muted pb-2 border-b border-cult-border">
                            <span>Total Votes Cast: <strong id="total-votes-count" class="text-white">0</strong></span>
                            <span>Target Status: <strong id="leader-name" class="text-cult-neon">None</strong></span>
                        </div>

                        <div id="tally-progress-container" class="space-y-4">
                            <!-- Live Progress Bars Dynamically Populated -->
                        </div>

                        <!-- Voter Log / Activity -->
                        <div class="pt-4 border-t border-cult-border">
                            <h3 class="text-xs font-semibold uppercase tracking-wider text-cult-muted mb-2">Voter Status Log</h3>
                            <div id="voter-status-list" class="space-y-1.5 text-xs max-h-40 overflow-y-auto pr-1">
                                <!-- Voters list -->
                            </div>
                        </div>
                    </div>
                </div>

            </div>
        </section>

    </main>

    <!-- Modal for Adding Candidate -->
    <div id="add-modal" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden items-center justify-center p-4">
        <div class="glass-panel max-w-md w-full p-6 rounded-xl border border-cult-neon/40 shadow-neon-glow space-y-4">
            <div class="flex items-center justify-between">
                <h3 class="text-lg font-cinzel font-bold text-white">Add Guy / Candidate</h3>
                <button onclick="closeAddCandidateModal()" class="text-cult-muted hover:text-white">
                    <i data-lucide="x" class="w-5 h-5"></i>
                </button>
            </div>
            <div>
                <label class="block text-xs uppercase tracking-wider text-cult-muted mb-1">Guy Name</label>
                <input type="text" id="new-candidate-name" placeholder="Enter candidate title/alias" 
                    class="w-full bg-cult-black border border-cult-border rounded-lg px-4 py-2 text-sm text-white focus:outline-none focus:border-cult-neon">
            </div>
            <div class="flex justify-end space-x-3 pt-2">
                <button onclick="closeAddCandidateModal()" class="px-4 py-2 rounded-lg text-xs text-cult-muted hover:text-white border border-cult-border">Cancel</button>
                <button onclick="submitAddCandidate()" class="px-4 py-2 rounded-lg text-xs bg-cult-neon text-white font-semibold hover:bg-red-600 shadow-neon-subtle">Add Nominee</button>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="border-t border-cult-border py-6 text-center text-xs text-cult-muted">
        <div class="max-w-6xl mx-auto px-4 flex flex-col sm:flex-row items-center justify-between gap-4">
            <span class="font-cinzel tracking-wider">BLACKCULT &copy; PRIVATE ANONYMOUS SYSTEM</span>
            <span class="text-cult-muted/60">Built for targeted group consensus. 1 Person = 1 Vote.</span>
        </div>
    </footer>

    <!-- Application Script Logic -->
    <script>
        // State storage (stored in localStorage for persistence across tabs/reloads)
        const STORAGE_KEY = 'BLACKCULT_GROUPS_DATA';
        const SESSION_KEY = 'BLACKCULT_CURRENT_SESSION';

        let currentSession = {
            groupId: null,
            userId: null,
            userName: null
        };

        // Initialize icons & app state
        window.onload = function() {
            lucide.createIcons();
            loadStateFromStorage();
            checkActiveSession();
        };

        // Get stored groups data
        function getGroupsData() {
            const data = localStorage.getItem(STORAGE_KEY);
            return data ? JSON.parse(data) : {};
        }

        // Save groups data
        function saveGroupsData(data) {
            localStorage.setItem(STORAGE_KEY, JSON.stringify(data));
        }

        // Generate custom room code
        function generateRoomCode() {
            const chars = 'ABCDEFGHJKLMNPQRSTUVWXYZ23456789';
            let code = 'CULT-';
            for (let i = 0; i < 4; i++) {
                code += chars.charAt(Math.floor(Math.random() * chars.length));
            }
            return code;
        }

        // Create Group Logic
        function handleCreateGroup() {
            const groupNameInput = document.getElementById('create-group-name').value.trim();
            const userNameInput = document.getElementById('create-user-name').value.trim();
            const candidatesInput = document.getElementById('create-candidates').value.trim();

            if (!groupNameInput || !userNameInput) {
                showToast('Please specify group title and your voter alias.', 'error');
                return;
            }

            const roomCode = generateRoomCode();
            const userId = 'usr_' + Math.random().toString(36).substr(2, 9);
            
            // Parse candidates list
            let candidateList = candidatesInput ? candidatesInput.split(',').map(c => c.trim()).filter(c => c.length > 0) : ['Guy 1', 'Guy 2'];
            
            // Format candidate objects
            const candidates = candidateList.map((name, index) => ({
                id: 'cand_' + index + '_' + Math.random().toString(36).substr(2, 5),
                name: name,
                votes: 0,
                voterIds: []
            }));

            const newGroup = {
                id: roomCode,
                name: groupNameInput,
                created: Date.now(),
                candidates: candidates,
                voters: [
                    { id: userId, name: userNameInput, hasVoted: false, votedFor: null }
                ]
            };

            const groups = getGroupsData();
            groups[roomCode] = newGroup;
            saveGroupsData(groups);

            // Set current session
            currentSession = { groupId: roomCode, userId: userId, userName: userNameInput };
            localStorage.setItem(SESSION_KEY, JSON.stringify(currentSession));

            showToast('Circle created successfully!', 'success');
            renderVotingDashboard();
            switchView('voting');
        }

        // Join Group Logic
        function handleJoinGroup() {
            const roomCodeInput = document.getElementById('join-group-code').value.trim().toUpperCase();
            const userNameInput = document.getElementById('join-user-name').value.trim();

            if (!roomCodeInput || !userNameInput) {
                showToast('Enter both group passcode and your alias.', 'error');
                return;
            }

            const groups = getGroupsData();
            const group = groups[roomCodeInput];

            if (!group) {
                showToast('Private circle not found. Check passcode.', 'error');
                return;
            }

            // Check if user already exists or create new voter ID
            let existingVoter = group.voters.find(v => v.name.toLowerCase() === userNameInput.toLowerCase());
            let userId;

            if (existingVoter) {
                userId = existingVoter.id;
            } else {
                userId = 'usr_' + Math.random().toString(36).substr(2, 9);
                group.voters.push({
                    id: userId,
                    name: userNameInput,
                    hasVoted: false,
                    votedFor: null
                });
                saveGroupsData(groups);
            }

            currentSession = { groupId: roomCodeInput, userId: userId, userName: userNameInput };
            localStorage.setItem(SESSION_KEY, JSON.stringify(currentSession));

            showToast(`Joined ${group.name}`, 'success');
            renderVotingDashboard();
            switchView('voting');
        }

        // Check active session on load
        function checkActiveSession() {
            const savedSession = localStorage.getItem(SESSION_KEY);
            if (savedSession) {
                try {
                    currentSession = JSON.parse(savedSession);
                    const groups = getGroupsData();
                    if (currentSession.groupId && groups[currentSession.groupId]) {
                        renderVotingDashboard();
                        switchView('voting');
                        return;
                    }
                } catch(e) {
                    console.error(e);
                }
            }
            switchView('landing');
        }

        // View Swapper
        function switchView(viewName) {
            const landingView = document.getElementById('view-landing');
            const votingView = document.getElementById('view-voting');
            const navInfo = document.getElementById('nav-session-info');

            if (viewName === 'voting') {
                landingView.classList.add('hidden');
                votingView.classList.remove('hidden');
                navInfo.classList.remove('hidden');
                navInfo.classList.add('flex');
            } else {
                votingView.classList.add('hidden');
                landingView.classList.remove('hidden');
                navInfo.classList.add('hidden');
                navInfo.classList.remove('flex');
            }
        }

        // Render Voting Dashboard
        function renderVotingDashboard() {
            const groups = getGroupsData();
            const group = groups[currentSession.groupId];

            if (!group) {
                leaveGroup();
                return;
            }

            const currentVoter = group.voters.find(v => v.id === currentSession.userId);
            const hasVoted = currentVoter ? currentVoter.hasVoted : false;

            // Header elements
            document.getElementById('group-display-title').innerText = group.name;
            document.getElementById('nav-group-name').innerText = group.name;
            document.getElementById('voter-display-name').innerText = currentSession.userName;
            document.getElementById('display-room-code').innerText = group.id;

            // Status Badge
            const statusBadge = document.getElementById('user-status-badge');
            if (hasVoted) {
                statusBadge.innerText = 'VOTE CAST';
                statusBadge.className = 'px-2.5 py-0.5 rounded text-xs uppercase tracking-wider font-semibold bg-emerald-500/10 text-emerald-400 border border-emerald-500/30';
            } else {
                statusBadge.innerText = 'VOTE PENDING (1 LEFT)';
                statusBadge.className = 'px-2.5 py-0.5 rounded text-xs uppercase tracking-wider font-semibold bg-yellow-500/10 text-yellow-500 border border-yellow-500/30';
            }

            // Render Candidate Cards
            const container = document.getElementById('candidates-container');
            container.innerHTML = '';

            if (group.candidates.length === 0) {
                container.innerHTML = `<div class="glass-panel p-6 rounded-xl text-center text-cult-muted text-sm">No members or candidates added yet. Click "Add Member" above.</div>`;
            } else {
                group.candidates.forEach(candidate => {
                    const isSelectedByMe = currentVoter && currentVoter.votedFor === candidate.id;

                    const card = document.createElement('div');
                    card.className = `glass-panel p-5 rounded-xl flex items-center justify-between transition-all duration-300 ${isSelectedByMe ? 'border-cult-neon shadow-neon-glow bg-cult-neon/5' : ''}`;
                    
                    card.innerHTML = `
                        <div class="flex items-center space-x-4">
                            <div class="w-10 h-10 rounded-full bg-cult-black border border-cult-border flex items-center justify-center font-cinzel font-bold text-cult-neon">
                                ${candidate.name.charAt(0).toUpperCase()}
                            </div>
                            <div>
                                <h3 class="font-semibold text-white text-base">${escapeHtml(candidate.name)}</h3>
                                <span class="text-xs text-cult-muted">${candidate.votes} ${candidate.votes === 1 ? 'vote' : 'votes'} received</span>
                            </div>
                        </div>

                        <div>
                            ${hasVoted ? `
                                <button disabled class="px-4 py-2 rounded-lg text-xs font-semibold uppercase tracking-wider ${isSelectedByMe ? 'bg-cult-neon text-white shadow-neon-subtle' : 'bg-cult-black border border-cult-border text-cult-muted opacity-50'} cursor-not-allowed">
                                    ${isSelectedByMe ? 'Voted' : 'Locked'}
                                </button>
                            ` : `
                                <button onclick="castVote('${candidate.id}')" class="px-5 py-2.5 rounded-lg text-xs font-semibold uppercase tracking-wider bg-cult-black border border-cult-neon text-cult-neon hover:bg-cult-neon hover:text-white transition shadow-neon-subtle flex items-center space-x-1.5">
                                    <i data-lucide="check-circle" class="w-4 h-4"></i>
                                    <span>Vote</span>
                                </button>
                            `}
                        </div>
                    `;
                    container.appendChild(card);
                });
            }

            // Render Tallies
            renderTallies(group);
            lucide.createIcons();
        }

        // Render Tally Bars and Leaderboard
        function renderTallies(group) {
            const totalVotes = group.candidates.reduce((sum, c) => sum + (c.votes || 0), 0);
            document.getElementById('total-votes-count').innerText = totalVotes;

            // Calculate leader
            let leader = null;
            let maxVotes = -1;
            group.candidates.forEach(c => {
                if (c.votes > maxVotes && c.votes > 0) {
                    maxVotes = c.votes;
                    leader = c.name;
                }
            });
            document.getElementById('leader-name').innerText = leader ? leader : 'Tie / None';

            const tallyContainer = document.getElementById('tally-progress-container');
            tallyContainer.innerHTML = '';

            // Sort candidates by vote count for live ranking
            const sortedCandidates = [...group.candidates].sort((a, b) => b.votes - a.votes);

            sortedCandidates.forEach(cand => {
                const percentage = totalVotes > 0 ? Math.round((cand.votes / totalVotes) * 100) : 0;
                
                const item = document.createElement('div');
                item.className = 'space-y-1.5';
                item.innerHTML = `
                    <div class="flex justify-between text-xs">
                        <span class="font-medium text-white">${escapeHtml(cand.name)}</span>
                        <span class="text-cult-muted">${cand.votes} (${percentage}%)</span>
                    </div>
                    <div class="w-full bg-cult-black h-2 rounded-full overflow-hidden border border-cult-border">
                        <div class="bg-gradient-to-r from-red-700 to-cult-neon h-full transition-all duration-500 rounded-full" style="width: ${percentage}%"></div>
                    </div>
                `;
                tallyContainer.appendChild(item);
            });

            // Voter Status List
            const voterList = document.getElementById('voter-status-list');
            voterList.innerHTML = '';
            group.voters.forEach(voter => {
                const vItem = document.createElement('div');
                vItem.className = 'flex items-center justify-between text-cult-muted py-1 border-b border-cult-border/40';
                vItem.innerHTML = `
                    <span class="flex items-center space-x-2">
                        <span class="w-1.5 h-1.5 rounded-full ${voter.hasVoted ? 'bg-emerald-500' : 'bg-yellow-500'}"></span>
                        <span class="${voter.id === currentSession.userId ? 'text-white font-medium' : ''}">${escapeHtml(voter.name)} ${voter.id === currentSession.userId ? '(You)' : ''}</span>
                    </span>
                    <span class="text-[10px] ${voter.hasVoted ? 'text-emerald-400' : 'text-yellow-500'} uppercase font-mono">
                        ${voter.hasVoted ? 'Done' : 'Pending'}
                    </span>
                `;
                voterList.appendChild(vItem);
            });
        }

        // Action: Cast Vote (1 Person 1 Vote enforcing)
        function castVote(candidateId) {
            const groups = getGroupsData();
            const group = groups[currentSession.groupId];

            if (!group) return;

            const voter = group.voters.find(v => v.id === currentSession.userId);
            if (!voter) {
                showToast('Voter session expired or invalid.', 'error');
                return;
            }

            if (voter.hasVoted) {
                showToast('You have already cast your 1 vote in this group.', 'error');
                return;
            }

            // Find target candidate
            const candidate = group.candidates.find(c => c.id === candidateId);
            if (candidate) {
                candidate.votes = (candidate.votes || 0) + 1;
                candidate.voterIds.push(currentSession.userId);
                
                voter.hasVoted = true;
                voter.votedFor = candidateId;

                saveGroupsData(groups);
                showToast(`Your vote has been sealed for ${candidate.name}`, 'success');
                renderVotingDashboard();
            }
        }

        // Reset Group Votes
        function resetGroupVotes() {
            if (!confirm('Are you sure you want to reset all vote counts and voter statuses in this circle?')) {
                return;
            }

            const groups = getGroupsData();
            const group = groups[currentSession.groupId];

            if (group) {
                group.candidates.forEach(c => {
                    c.votes = 0;
                    c.voterIds = [];
                });
                group.voters.forEach(v => {
                    v.hasVoted = false;
                    v.votedFor = null;
                });

                saveGroupsData(groups);
                showToast('Voting session reset. All members can vote again.', 'info');
                renderVotingDashboard();
            }
        }

        // Candidate Modal Handlers
        function openAddCandidateModal() {
            document.getElementById('add-modal').classList.remove('hidden');
            document.getElementById('add-modal').classList.add('flex');
        }

        function closeAddCandidateModal() {
            document.getElementById('add-modal').classList.add('hidden');
            document.getElementById('add-modal').classList.remove('flex');
            document.getElementById('new-candidate-name').value = '';
        }

        function submitAddCandidate() {
            const nameInput = document.getElementById('new-candidate-name').value.trim();
            if (!nameInput) {
                showToast('Please enter candidate name.', 'error');
                return;
            }

            const groups = getGroupsData();
            const group = groups[currentSession.groupId];

            if (group) {
                const newCand = {
                    id: 'cand_' + group.candidates.length + '_' + Math.random().toString(36).substr(2, 5),
                    name: nameInput,
                    votes: 0,
                    voterIds: []
                };

                group.candidates.push(newCand);
                saveGroupsData(groups);
                showToast(`Added ${nameInput} to ballot`, 'success');
                closeAddCandidateModal();
                renderVotingDashboard();
            }
        }

        // Copy Code to Clipboard
        function copyGroupCode() {
            const code = document.getElementById('display-room-code').innerText;
            const tempInput = document.createElement('input');
            tempInput.value = code;
            document.body.appendChild(tempInput);
            tempInput.select();
            document.execCommand('copy');
            document.body.removeChild(tempInput);
            showToast('Passcode copied to clipboard!', 'info');
        }

        // Leave current group
        function leaveGroup() {
            localStorage.removeItem(SESSION_KEY);
            currentSession = { groupId: null, userId: null, userName: null };
            switchView('landing');
            showToast('Exited private session.', 'info');
        }

        // Toast notification system
        function showToast(message, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            
            let borderClass = 'border-cult-neon text-white';
            if (type === 'error') borderClass = 'border-red-600 text-red-200';
            if (type === 'success') borderClass = 'border-emerald-500 text-emerald-200';

            toast.className = `glass-panel px-4 py-3 rounded-lg border ${borderClass} shadow-lg text-xs font-medium flex items-center space-x-2 transition-all duration-300 transform translate-y-2 opacity-0 pointer-events-auto`;
            toast.innerHTML = `
                <i data-lucide="${type === 'success' ? 'check-circle' : type === 'error' ? 'alert-circle' : 'info'}" class="w-4 h-4 text-cult-neon"></i>
                <span>${escapeHtml(message)}</span>
            `;

            container.appendChild(toast);
            lucide.createIcons();

            setTimeout(() => {
                toast.classList.remove('translate-y-2', 'opacity-0');
            }, 10);

            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-2');
                setTimeout(() => {
                    toast.remove();
                }, 300);
            }, 3000);
        }

        // Utility: Escape HTML strings
        function escapeHtml(str) {
            return String(str).replace(/[&<>"']/g, function(m) {
                return {
                    '&': '&amp;',
                    '<': '&lt;',
                    '>': '&gt;',
                    '"': '&quot;',
                    "'": '&#039;'
                }[m];
            });
        }
    </script>
</body>
</html>
