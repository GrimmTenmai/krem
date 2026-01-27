//бургер
document.addEventListener('DOMContentLoaded', function () {
  const burgerMenu = document.getElementById('burgerMenu');
  const menuOverlay = document.getElementById('menuOverlay');
  const menuLinks = menuOverlay.querySelectorAll('a');
  function toggleMenu() {
    burgerMenu.classList.toggle('active');
    menuOverlay.classList.toggle('active');
    if (menuOverlay.classList.contains('active')) {
      menuOverlay.setAttribute('aria-hidden', 'false');
      document.body.style.overflow = 'hidden';
    } else {
      menuOverlay.setAttribute('aria-hidden', 'true');
      document.body.style.overflow = '';
    }
  }
  burgerMenu.addEventListener('click', toggleMenu);
  menuLinks.forEach(link => {
    link.addEventListener('click', function (e) {
      e.preventDefault();
      const targetId = this.getAttribute('href').substring(1);
      const targetElement = document.getElementById(targetId);
      if (targetElement) {
        targetElement.scrollIntoView({ behavior: 'smooth' });
      }

      burgerMenu.classList.remove('active');
      menuOverlay.classList.remove('active');
      menuOverlay.setAttribute('aria-hidden', 'true');
      document.body.style.overflow = '';
    });
  });
});

// Функция открытия сайдбара
function openSidebar() {
    const sidebar = document.getElementById('sidebar');
    sidebar.classList.add('open');
    sidebar.removeAttribute('aria-hidden');
    sidebar.removeAttribute('inert');
    
    // Установите фокус на кнопку закрытия
    document.getElementById('closeMenu').focus();
}

// Функция закрытия сайдбара
function closeSidebar() {
    const sidebar = document.getElementById('sidebar');
    sidebar.classList.remove('open');
    sidebar.setAttribute('aria-hidden', 'true');
    sidebar.setAttribute('inert', '');

    // Верните фокус на основной контент
    document.querySelector('main').focus();
}

// Назначьте обработчики событий
document.getElementById('closeMenu').addEventListener('click', closeSidebar);
document.getElementById('closeMenu').addEventListener('keydown', (e) => {
    if (e.key === 'Enter' || e.key === ' ') closeSidebar();
});

document.addEventListener('DOMContentLoaded', function() {
    let favorites = JSON.parse(localStorage.getItem('favorites')) || [];

    // Обработчик для сердечек
    document.querySelectorAll('.favorites__bg').forEach(btn => {
        btn.addEventListener('click', function(e) {
            e.preventDefault();
            e.stopPropagation();
            
            const item = this.closest('.catalog__item');
            const title = item.querySelector('p:first-child').textContent;
            const price = item.querySelector('p:last-child').textContent;
            
            // Изменено на favorites-active
            this.classList.toggle('favorites-active');
            
            const index = favorites.findIndex(fav => fav.title === title);
            if (index === -1) {
                favorites.push({ title, price });
            } else {
                favorites.splice(index, 1);
            }
            
            localStorage.setItem('favorites', JSON.stringify(favorites));
        });
    });

    // Обновление состояния сердечек
    document.querySelectorAll('.catalog__item').forEach(item => {
        const title = item.querySelector('p:first-child').textContent;
        if (favorites.some(fav => fav.title === title)) {
            item.querySelector('.favorites__bg').classList.add('favorites-active');
        }
    });

    // Остальной код без изменений
    window.openSidebar = function() {
        const sidebar = document.getElementById('sidebar');
        sidebar.style.display = 'block';
        updateFavoritesList();
    };

    function updateFavoritesList() {
        const list = document.getElementById('favorites-list');
        list.innerHTML = '';
        
        favorites.forEach(item => {
            const li = document.createElement('li');
            li.className = 'favorites-item';
            li.innerHTML = `
                <p>${item.title}</p>
                <p>${item.price}</p>
            `;
            list.appendChild(li);
        });
    }

    document.getElementById('closeMenu').addEventListener('click', function() {
        document.getElementById('sidebar').style.display = 'none';
    });
});