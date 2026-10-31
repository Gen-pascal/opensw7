<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>SCH 룸메이팅 - 순천향대 익명 룸메이트 찾기</title>
  <script src="https://cdn.jsdelivr.net/npm/@supabase/supabase-js@2"></script>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-gray-50 min-h-screen text-gray-800">
  <!-- 상단 헤더 -->
  <header class="bg-blue-600 text-white p-4 shadow-md sticky top-0 z-10">
    <div class="max-w-4xl mx-auto flex justify-between items-center">
      <h1 class="text-xl font-bold flex items-center gap-2">🏫 순천향대 익명 룸메이팅</h1>
      <div id="auth-buttons" class="flex items-center gap-2">
        <button onclick="openAuthModal('login')" class="bg-blue-500 hover:bg-blue-700 px-3 py-1.5 rounded-md font-medium text-sm transition">로그인</button>
        <button onclick="openAuthModal('signup')" class="bg-white text-blue-600 hover:bg-blue-50 px-3 py-1.5 rounded-md font-bold text-sm transition">회원가입</button>
      </div>
    </div>
  </header>

  <main class="max-w-4xl mx-auto p-4">
    <!-- 기숙사 선택 필터 탭 -->
    <section class="mb-6">
      <div class="flex justify-between items-center mb-2">
        <h2 class="text-sm font-semibold text-gray-500">기숙사 선택</h2>
        <button onclick="toggleForm()" class="bg-blue-600 text-white px-3 py-1.5 rounded-md font-bold text-sm hover:bg-blue-700 transition">
          + 조건 글쓰기
        </button>
      </div>
      <div class="flex gap-2 overflow-x-auto pb-2 scrollbar-hide" id="dorm-tabs">
        <button onclick="selectDorm('전체')" class="dorm-btn px-4 py-2 rounded-full border bg-blue-600 text-white text-sm font-medium whitespace-nowrap">전체</button>
        <button onclick="selectDorm('학성사1')" class="dorm-btn px-4 py-2 rounded-full border bg-white text-gray-700 text-sm font-medium whitespace-nowrap hover:bg-gray-100">학성사1</button>
        <button onclick="selectDorm('학성사2')" class="dorm-btn px-4 py-2 rounded-full border bg-white text-gray-700 text-sm font-medium whitespace-nowrap hover:bg-gray-100">학성사2</button>
        <button onclick="selectDorm('학성사3')" class="dorm-btn px-4 py-2 rounded-full border bg-white text-gray-700 text-sm font-medium whitespace-nowrap hover:bg-gray-100">학성사3</button>
        <button onclick="selectDorm('향설생활관 1관')" class="dorm-btn px-4 py-2 rounded-full border bg-white text-gray-700 text-sm font-medium whitespace-nowrap hover:bg-gray-100">향설생활관 1관</button>
        <button onclick="selectDorm('향설생활관 2관')" class="dorm-btn px-4 py-2 rounded-full border bg-white text-gray-700 text-sm font-medium whitespace-nowrap hover:bg-gray-100">향설생활관 2관</button>
        <button onclick="selectDorm('향설생활관 3관')" class="dorm-btn px-4 py-2 rounded-full border bg-white text-gray-700 text-sm font-medium whitespace-nowrap hover:bg-gray-100">향설생활관 3관</button>
      </div>
    </section>

    <!-- 구인글 작성 폼 -->
    <section id="post-form" class="hidden bg-white p-6 rounded-xl shadow-lg mb-6 border border-gray-200">
      <h2 class="text-lg font-bold mb-4 text-gray-900 border-b pb-2">룸메이트 구인글 작성</h2>
      <form id="roommate-form" onsubmit="submitPost(event)" class="space-y-4">
        <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">거주 기숙사 *</label>
            <select id="form-dormitory" required class="w-full border rounded-lg p-2.5 bg-gray-50 focus:bg-white focus:ring-2 focus:ring-blue-500 outline-none">
              <option value="">기숙사를 선택하세요</option>
              <option value="학성사1">학성사1</option>
              <option value="학성사2">학성사2</option>
              <option value="학성사3">학성사3</option>
              <option value="향설생활관 1관">향설생활관 1관</option>
              <option value="향설생활관 2관">향설생활관 2관</option>
              <option value="향설생활관 3관">향설생활관 3관</option>
            </select>
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">익명 닉네임 *</label>
            <input type="text" id="nickname" required class="w-full border rounded-lg p-2.5 bg-gray-50 focus:bg-white focus:ring-2 focus:ring-blue-500 outline-none" placeholder="예: 향설2관새내기">
          </div>
        </div>

        <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">성별</label>
            <select id="gender" class="w-full border rounded-lg p-2.5 bg-gray-50 focus:bg-white focus:ring-2 focus:ring-blue-500 outline-none">
              <option value="male">남성</option>
              <option value="female">여성</option>
            </select>
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">흡연 여부</label>
            <select id="is_smoker" class="w-full border rounded-lg p-2.5 bg-gray-50 focus:bg-white focus:ring-2 focus:ring-blue-500 outline-none">
              <option value="false">비흡연</option>
              <option value="true">흡연</option>
            </select>
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">코골이/이갈이</label>
            <select id="snoring" class="w-full border rounded-lg p-2.5 bg-gray-50 focus:bg-white focus:ring-2 focus:ring-blue-500 outline-none">
              <option value="false">없음</option>
              <option value="true">있음</option>
            </select>
          </div>
          <div>
            <label class="block text-sm font-medium text-gray-700 mb-1">침대 선호</label>
            <select id="bed" class="w-full border rounded-lg p-2.5 bg-gray-50 focus:bg-white focus:ring-2 focus:ring-blue-500 outline-none">
              <option value="up">상층 (2층)</option>
              <option value="down">하층 (1층)</option>
              <option value="any">무관</option>
            </select>
          </div>
        </div>

        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">평균 기상 시간</label>
          <input type="text" id="wake_time" placeholder="예: 08:00 AM" class="w-full border rounded-lg p-2.5 bg-gray-50 focus:bg-white focus:ring-2 focus:ring-blue-500 outline-none">
        </div>

        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">추가 설명 / 생활 패턴</label>
          <textarea id="description" rows="3" class="w-full border rounded-lg p-2.5 bg-gray-50 focus:bg-white focus:ring-2 focus:ring-blue-500 outline-none" placeholder="취침 시간, 소음 민감도, 청소 주기 등 자유롭게 작성하세요."></textarea>
        </div>

        <div class="flex justify-end gap-2 pt-2">
          <button type="button" onclick="toggleForm()" class="px-4 py-2 border rounded-lg hover:bg-gray-50 text-gray-600 font-medium">취소</button>
          <button type="submit" class="px-5 py-2 bg-blue-600 hover:bg-blue-700 text-white font-medium rounded-lg shadow-sm">등록하기</button>
        </div>
      </form>
    </section>

    <!-- 게시글 목록 영역 -->
    <section>
      <div class="flex justify-between items-center mb-4">
        <h2 id="current-dorm-title" class="text-lg font-bold text-gray-800">전체 게시글</h2>
      </div>
      <div id="posts-container" class="grid gap-4 md:grid-cols-2"></div>
    </section>
  </main>

  <!-- 로그인 / 회원가입 모달 -->
  <div id="auth-modal" class="hidden fixed inset-0 bg-black/50 z-50 flex items-center justify-center p-4">
    <div class="bg-white rounded-xl shadow-xl w-full max-w-md p-6 relative">
      <button onclick="closeAuthModal()" class="absolute top-4 right-4 text-gray-400 hover:text-gray-600">✕</button>
      <h3 id="modal-title" class="text-xl font-bold text-gray-900 mb-4">로그인</h3>
      <form id="auth-form" onsubmit="handleAuthSubmit(event)" class="space-y-4">
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">아이디</label>
          <input type="text" id="auth-username" required class="w-full border rounded-lg p-2.5 bg-gray-50 focus:bg-white outline-none" placeholder="아이디 입력">
        </div>
        <div>
          <label class="block text-sm font-medium text-gray-700 mb-1">비밀번호</label>
          <input type="password" id="auth-password" required class="w-full border rounded-lg p-2.5 bg-gray-50 focus:bg-white outline-none" placeholder="비밀번호 입력">
        </div>
        <button type="submit" id="auth-submit-btn" class="w-full bg-blue-600 text-white py-3 rounded-lg font-bold hover:bg-blue-700 transition">로그인</button>
      </form>
    </div>
  </div>

  <script>
    const SUPABASE_URL = 'https://opdodufamsaqtvnbjekr.supabase.co';
    const SUPABASE_KEY = 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6Im9wZG9kdWZhbXNhcXR2bmJqZWtyIiwicm9sZSI6ImFub24iLCJpYXQiOjE3OTM0MzY2NjIsImV4cCI6MjEwOTAxMjY2Mn0.FF5Dthys5_Sj6ufoKuvlyQaPkKcs9kN2FhNJsR60TOg';
    const supabaseClient = supabase.createClient(SUPABASE_URL, SUPABASE_KEY);

    let currentSelectedDorm = '전체';
    let authMode = 'login';
    let currentUser = null;

    document.addEventListener('DOMContentLoaded', () => {
      checkLoginStatus();
      fetchPosts();
    });

    function checkLoginStatus() {
      const savedUser = localStorage.getItem('sch_roommate_user');
      if (savedUser) {
        currentUser = JSON.parse(savedUser);
        renderAuthButtons();
      }
    }

    function renderAuthButtons() {
      const container = document.getElementById('auth-buttons');
      if (currentUser) {
        container.innerHTML = `
          <span class="text-sm font-semibold bg-blue-700 px-3 py-1.5 rounded-md">👤 ${escapeHtml(currentUser.username)}</span>
          <button onclick="logout()" class="bg-blue-500 hover:bg-blue-700 px-3 py-1.5 rounded-md font-medium text-sm transition">로그아웃</button>
        `;
      } else {
        container.innerHTML = `
          <button onclick="openAuthModal('login')" class="bg-blue-500 hover:bg-blue-700 px-3 py-1.5 rounded-md font-medium text-sm transition">로그인</button>
          <button onclick="openAuthModal('signup')" class="bg-white text-blue-600 hover:bg-blue-50 px-3 py-1.5 rounded-md font-bold text-sm transition">회원가입</button>
        `;
      }
    }

    function openAuthModal(mode) {
      authMode = mode;
      document.getElementById('modal-title').innerText = mode === 'login' ? '로그인' : '회원가입';
      document.getElementById('auth-submit-btn').innerText = mode === 'login' ? '로그인' : '회원가입';
      document.getElementById('auth-modal').classList.remove('hidden');
    }

    function closeAuthModal() {
      document.getElementById('auth-modal').classList.add('hidden');
      document.getElementById('auth-form').reset();
    }

    async function handleAuthSubmit(e) {
      e.preventDefault();
      const username = document.getElementById('auth-username').value.trim();
      const password = document.getElementById('auth-password').value.trim();

      if (!username || !password) return alert('아이디와 비밀번호를 모두 입력해주세요.');

      if (authMode === 'signup') {
        // 1. 중복 아이디 검사
        const { data: existing } = await supabaseClient.from('users').select('*').eq('username', username);
        if (existing && existing.length > 0) {
          return alert('이미 존재하는 아이디입니다.');
        }

        // 2. Supabase users 테이블에 계정 추가
        const { data, error } = await supabaseClient.from('users').insert([{ username, password }]).select();
        
        if (error) {
          alert('회원가입 오류: ' + error.message);
        } else {
          alert('회원가입이 완료되었습니다! 로그인해주세요.');
          openAuthModal('login');
        }
      } else {
        // 로그인 처리
        const { data, error } = await supabaseClient.from('users').select('*').eq('username', username).eq('password', password);
        
        if (error || !data || data.length === 0) {
          alert('아이디 또는 비밀번호가 일치하지 않습니다.');
        } else {
          currentUser = data[0];
          localStorage.setItem('sch_roommate_user', JSON.stringify(currentUser));
          renderAuthButtons();
          closeAuthModal();
          alert(`${currentUser.username}님, 환영합니다!`);
        }
      }
    }

    function logout() {
      localStorage.removeItem('sch_roommate_user');
      currentUser = null;
      renderAuthButtons();
      alert('로그아웃 되었습니다.');
    }

    function toggleForm() {
      document.getElementById('post-form').classList.toggle('hidden');
    }

    function selectDorm(dormName) {
      currentSelectedDorm = dormName;
      document.querySelectorAll('.dorm-btn').forEach(btn => {
        btn.className = btn.innerText === dormName 
          ? 'dorm-btn px-4 py-2 rounded-full border bg-blue-600 text-white text-sm font-medium whitespace-nowrap'
          : 'dorm-btn px-4 py-2 rounded-full border bg-white text-gray-700 text-sm font-medium whitespace-nowrap hover:bg-gray-100';
      });
      document.getElementById('current-dorm-title').innerText = `${dormName} 게시글`;
      fetchPosts();
    }

    async function fetchPosts() {
      let query = supabaseClient.from('posts').select('*').order('created_at', { ascending: false });
      if (currentSelectedDorm !== '전체') {
        query = query.eq('dormitory', currentSelectedDorm);
      }

      const { data: posts, error } = await query;
      const container = document.getElementById('posts-container');
      container.innerHTML = '';

      if (error || !posts || posts.length === 0) {
        container.innerHTML = `<div class="col-span-2 text-center py-12 text-gray-400 bg-white rounded-xl border">등록된 게시글이 없습니다. 첫 글을 작성해보세요!</div>`;
        return;
      }

      posts.forEach(post => {
        const card = document.createElement('div');
        card.className = 'bg-white p-5 rounded-xl shadow-sm border border-gray-200 space-y-3';
        card.innerHTML = `
          <div class="flex justify-between items-start">
            <div>
              <span class="inline-block bg-blue-100 text-blue-700 text-xs px-2 py-0.5 rounded font-semibold mb-1">${escapeHtml(post.dormitory)}</span>
              <h3 class="font-bold text-gray-900">${escapeHtml(post.nickname)}</h3>
            </div>
            <span class="text-xs text-gray-400">${new Date(post.created_at).toLocaleDateString()}</span>
          </div>
          <div class="grid grid-cols-2 gap-2 text-xs bg-gray-50 p-3 rounded-lg text-gray-600">
            <div>🚬 흡연: <b>${post.is_smoker ? '흡연' : '비흡연'}</b></div>
            <div>😴 코골이: <b>${post.snoring ? '있음' : '없음'}</b></div>
            <div>🛌 침대: <b>${post.bed_preference === 'up' ? '2층' : post.bed_preference === 'down' ? '1층' : '무관'}</b></div>
            <div>⏰ 기상: <b>${escapeHtml(post.wake_time || '미지정')}</b></div>
          </div>
          <p class="text-sm text-gray-700">${escapeHtml(post.description)}</p>
          <button onclick="alert('1대1 채팅이 연결됩니다.')" class="w-full bg-blue-50 text-blue-600 hover:bg-blue-100 py-2 rounded-lg font-semibold text-sm transition">
            💬 1:1 익명 채팅하기
          </button>
        `;
        container.appendChild(card);
      });
    }

    async function submitPost(e) {
      e.preventDefault();
      const newPost = {
        dormitory: document.getElementById('form-dormitory').value,
        nickname: document.getElementById('nickname').value,
        gender: document.getElementById('gender').value,
        is_smoker: document.getElementById('is_smoker').value === 'true',
        snoring: document.getElementById('snoring').value === 'true',
        bed_preference: document.getElementById('bed').value,
        wake_time: document.getElementById('wake_time').value,
        description: document.getElementById('description').value
      };

      const { error } = await supabaseClient.from('posts').insert([newPost]);
      if (error) {
        alert('글 등록 실패: ' + error.message);
      } else {
        document.getElementById('roommate-form').reset();
        toggleForm();
        fetchPosts();
      }
    }

    function escapeHtml(text) {
      return text ? text.replace(/&/g, "&amp;").replace(/</g, "&lt;").replace(/>/g, "&gt;") : '';
    }
  </script>
</body>
</html>
