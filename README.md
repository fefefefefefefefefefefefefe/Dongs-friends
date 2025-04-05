# Dongs-friends
동서울대학교 졸업작품


<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>중앙 피드 SNS</title>
  <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-pink-50 text-pink-500 relative">

  <!-- 상단 고정 헤더 -->
  <header class="fixed top-0 left-0 w-full bg-white shadow-md z-50">
    <div class="max-w-screen-sm mx-auto flex items-center justify-between px-4 py-3">
      <span class="text-lg font-bold">DongStory</span>
    </div>
  </header>

  <!-- 하얀 고정 박스 -->
  <div class="pt-20 pb-28 flex justify-center">
    <div class="bg-white w-full max-w-screen-sm min-h-[calc(100vh-112px)] rounded-3xl shadow border border-pink-200 relative overflow-hidden">

      <!-- 피드만 스크롤 가능 -->
      <div class="overflow-y-auto h-full p-4 space-y-6">

        <!-- 피드 카드 -->
        <div class="bg-white border border-pink-100 rounded-3xl p-4 shadow w-full">
          <!-- 사용자 정보 -->
          <div class="flex items-center mb-3">
            <img src="https://randomuser.me/api/portraits/women/30.jpg" class="w-10 h-10 rounded-full object-cover mr-3" />
            <span class="font-semibold text-pink-500">sumin_lee</span>
          </div>

          <!-- 이미지 슬라이드 영역 -->
          <div class="relative w-full overflow-hidden mb-2">
            <div class="flex w-full overflow-x-auto scroll-smooth snap-x snap-mandatory rounded-3xl" id="imageSlider">
              <div class="flex-shrink-0 w-full aspect-square snap-start">
                <img src="https://ifh.cc/g/KkDLXS.jpg" class="w-full h-full object-cover rounded-3xl" />
              </div>
              <div class="flex-shrink-0 w-full aspect-square snap-start">
                <img src="https://source.unsplash.com/random/800x800?nature" class="w-full h-full object-cover rounded-3xl" />
              </div>
              <div class="flex-shrink-0 w-full aspect-square snap-start">
                <img src="https://source.unsplash.com/random/800x800?cat" class="w-full h-full object-cover rounded-3xl" />
              </div>
              <div class="flex-shrink-0 w-full aspect-square snap-start">
                <img src="https://source.unsplash.com/random/800x800?dog" class="w-full h-full object-cover rounded-3xl" />
              </div>
              <div class="flex-shrink-0 w-full aspect-square snap-start">
                <img src="https://source.unsplash.com/random/800x800?flower" class="w-full h-full object-cover rounded-3xl" />
              </div>
            </div>
            <button onclick="scrollSlider('left')" class="absolute top-1/2 left-2 -translate-y-1/2 bg-white rounded-full p-2 shadow-md z-10">
              <span class="text-black text-xl">←</span>
            </button>
            <button onclick="scrollSlider('right')" class="absolute top-1/2 right-2 -translate-y-1/2 bg-white rounded-full p-2 shadow-md z-10">
              <span class="text-black text-xl">→</span>
            </button>
          </div>

          <!-- 좋아요 & 댓글 -->
          <div class="flex items-center space-x-4 text-lg">
            <button onclick="likePost(this)" class="flex items-center space-x-1">
              <span>❤️</span><span class="like-count">0</span>
            </button>
            <button onclick="toggleComment(this)">💬 댓글</button>
          </div>
          <p class="text-sm text-gray-600 mt-1 like-text">좋아요 0개</p>
        </div>

      </div>
    </div>
  </div>

  <!-- 하단 고정 네비게이션 바 -->
  <nav class="fixed bottom-0 left-0 w-full bg-white shadow-md border-t border-gray-200 z-50">
    <div class="max-w-screen-sm mx-auto flex justify-around items-center py-3">
      <button class="text-2xl">🏠</button>
      <button class="text-2xl">💬</button>
      <img src="https://randomuser.me/api/portraits/women/30.jpg" alt="프로필" class="w-8 h-8 rounded-full object-cover border-2 border-pink-300" />
      <button class="text-2xl">⚙️</button>
      <button class="text-2xl">☰</button>
    </div>
  </nav>

  <!-- JS 기능들 -->
  <script>
    function likePost(button) {
      const countSpan = button.querySelector('.like-count');
      const textSpan = button.closest('div').nextElementSibling;
      let count = parseInt(countSpan.textContent);
      count++;
      countSpan.textContent = count;
      textSpan.textContent = `좋아요 ${count}개`;
      button.disabled = true;
      button.classList.add('opacity-50', 'cursor-not-allowed');
    }

    function scrollSlider(direction) {
      const slider = document.getElementById('imageSlider');
      const scrollAmount = slider.offsetWidth;
      if (direction === 'left') {
        slider.scrollBy({ left: -scrollAmount, behavior: 'smooth' });
      } else {
        slider.scrollBy({ left: scrollAmount, behavior: 'smooth' });
      }
    }

    function toggleComment(button) {
      alert("댓글 기능은 곧 추가될 예정입니다!");
    }
  </script>
</body>
<script type="module">
  import { initializeApp } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-app.js";
  import { getFirestore, collection, getDocs } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-firestore.js";
  import { getAnalytics } from "https://www.gstatic.com/firebasejs/10.7.1/firebase-analytics.js";
  import { getAuth, onAuthStateChanged } from "firebase/auth";

  onAuthStateChanged(auth, (user) => {
    if (!user) {
      window.location.href = "login.html";
    }
  });

  const firebaseConfig = {
    apiKey: "AIzaSyD2aaHhJvjqaxj9SYjDY05y0TYiEAa8vmc",
    authDomain: "dongstory-e5a28.firebaseapp.com",
    projectId: "dongstory-e5a28",
    storageBucket: "dongstory-e5a28.firebasestorage.app",
    messagingSenderId: "835308091634",
    appId: "1:835308091634:web:24afe4c5ac6885e58a786b",
    measurementId: "G-Q06TPW85W0"
  };

  const app = initializeApp(firebaseConfig);
  const db = getFirestore(app);
  const analytics = getAnalytics(app);

  // ✅ Firestore에서 데이터 불러오기 테스트
  const querySnapshot = await getDocs(collection(db, "posts"));
  querySnapshot.forEach((doc) => {
    console.log(doc.id, " => ", doc.data());
  });
</script>

  // 나중에 버튼 클릭으로 게시글 추가할 때 사용 가능
  // await addDoc(collection(db, "posts"), {
  //   username: "sumin_lee",
  //   likes: 0,
  //   createdAt: new Date()
  // });
</html>
