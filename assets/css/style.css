'use strict';



// element toggle function
const elementToggleFunc = function (elem) { elem.classList.toggle("active"); }



// sidebar variables
const sidebar = document.querySelector("[data-sidebar]");
const sidebarBtn = document.querySelector("[data-sidebar-btn]");

// sidebar toggle functionality for mobile
sidebarBtn.addEventListener("click", function () { elementToggleFunc(sidebar); });



// testimonials variables
const testimonialsItem = document.querySelectorAll("[data-testimonials-item]");
const modalContainer = document.querySelector("[data-modal-container]");
const modalCloseBtn = document.querySelector("[data-modal-close-btn]");
const overlay = document.querySelector("[data-overlay]");

// modal variable
const modalImg = document.querySelector("[data-modal-img]");
const modalTitle = document.querySelector("[data-modal-title]");
const modalText = document.querySelector("[data-modal-text]");

// project modal variables
const projectItem = document.querySelectorAll("[data-project-item]");
const projectModalContainer = document.querySelector("[data-project-modal-container]");
const projectModalCloseBtn = document.querySelector("[data-project-modal-close-btn]");
const projectOverlay = document.querySelector("[data-project-overlay]");

// project modal variable
const projectModalImg = document.querySelector("[data-project-modal-img]");
const projectModalTitle = document.querySelector("[data-project-modal-title]");
const projectModalDescription = document.querySelector("[data-project-modal-description]");

let currentScale = 1;
let translateX = 0;
let translateY = 0;
let maxScale = 1;

const imgWrapper = projectModalContainer.querySelector(".project-modal-img-wrapper");

function applyTransform(noTransition = false) {
  if (noTransition) {
    projectModalImg.style.transition = 'none';
  } else {
    projectModalImg.style.transition = 'transform 0.2s ease';
  }

  projectModalImg.style.transformOrigin = "center center";

  // Get latest dimensions to handle any layout changes
  const baseWidth = projectModalImg.clientWidth;
  const baseHeight = projectModalImg.clientHeight;
  const wrapperWidth = imgWrapper.clientWidth;
  const wrapperHeight = imgWrapper.clientHeight;

  if (baseWidth === 0 || baseHeight === 0) return;

  const zoomedWidth = baseWidth * currentScale;
  const zoomedHeight = baseHeight * currentScale;

  // Strict boundary clamping
  let maxOffsetX = Math.max(0, (zoomedWidth - wrapperWidth) / 2);
  let maxOffsetY = Math.max(0, (zoomedHeight - wrapperHeight) / 2);

  translateX = Math.min(Math.max(translateX, -maxOffsetX), maxOffsetX);
  translateY = Math.min(Math.max(translateY, -maxOffsetY), maxOffsetY);

  // We use translateX/Y as screen pixels. 
  // In CSS transform: translate() scale(), the translate is applied in the parent's coordinate space.
  projectModalImg.style.transform = `translate(${translateX}px, ${translateY}px) scale(${currentScale})`;
}

function resetZoom() {
  currentScale = 1;
  translateX = 0;
  translateY = 0;
  projectModalImg.style.transition = 'transform 0.4s ease';
  projectModalImg.style.transform = '';
  imgWrapper.classList.remove("zoomed");
}

// project modal toggle function
const projectModalFunc = function () {
  const isActive = projectModalContainer.classList.contains("active");
  projectModalContainer.classList.toggle("active");
  projectOverlay.classList.toggle("active");

  if (isActive) {
    resetZoom();
  }
}

// add click event to all project items
for (let i = 0; i < projectItem.length; i++) {
  projectItem[i].addEventListener("click", function (event) {
    event.preventDefault();

    projectModalImg.src = this.querySelector("[data-project-img]").src;
    projectModalImg.alt = this.querySelector("[data-project-img]").alt;
    projectModalTitle.innerHTML = this.dataset.projectTitle;
    projectModalDescription.innerHTML = this.dataset.projectDescription;

    resetZoom();

    projectModalImg.onload = function () {
      // Ensure layout is ready
      setTimeout(() => {
        if (projectModalImg.clientWidth > 0) {
          maxScale = Math.max(1, projectModalImg.naturalWidth / projectModalImg.clientWidth);
        } else {
          maxScale = 3; // Fallback
        }
      }, 100);
    };

    projectModalFunc();
  });
}

// Scroll to zoom
imgWrapper.addEventListener("wheel", function (event) {
  event.preventDefault();
  event.stopPropagation();

  const zoomStep = 0.2;
  const oldScale = currentScale;

  if (event.deltaY < 0) {
    currentScale += zoomStep;
  } else {
    currentScale -= zoomStep;
  }

  currentScale = Math.min(Math.max(1.0, currentScale), maxScale);

  if (currentScale <= 1.01) { // Small threshold to avoid floating point issues
    resetZoom();
    return;
  }

  // Calculate cursor offset for zoom-at-point
  const rect = projectModalImg.getBoundingClientRect();
  const mouseX = event.clientX - (rect.left + rect.width / 2);
  const mouseY = event.clientY - (rect.top + rect.height / 2);

  const ratio = 1 - currentScale / oldScale;
  translateX += mouseX * ratio;
  translateY += mouseY * ratio;

  imgWrapper.classList.add("zoomed");
  applyTransform(true); // No transition during scroll for precision
}, { passive: false });

// Click and drag panning
let isDragging = false;
let dragStartX, dragStartY, dragStartTransX, dragStartTransY;

imgWrapper.addEventListener("mousedown", (e) => {
  if (currentScale <= 1.01) return;
  e.preventDefault();
  isDragging = true;
  imgWrapper.classList.add("dragging");
  dragStartX = e.clientX;
  dragStartY = e.clientY;
  dragStartTransX = translateX;
  dragStartTransY = translateY;
});

document.addEventListener("mousemove", (e) => {
  if (!isDragging) return;
  translateX = dragStartTransX + (e.clientX - dragStartX);
  translateY = dragStartTransY + (e.clientY - dragStartY);
  applyTransform(true); // Instant response during drag
});

document.addEventListener("mouseup", () => {
  if (!isDragging) return;
  isDragging = false;
  imgWrapper.classList.remove("dragging");
  projectModalImg.style.transition = 'transform 0.2s ease';
});

// Global escape key listener
document.addEventListener("keydown", (e) => {
  if (e.key === "Escape" && projectModalContainer.classList.contains("active")) {
    projectModalFunc();
  }
});

// Close modal on close button or overlay click
projectModalCloseBtn.addEventListener("click", projectModalFunc);
projectOverlay.addEventListener("click", projectModalFunc);

// modal toggle function
const testimonialsModalFunc = function () {
  modalContainer.classList.toggle("active");
  overlay.classList.toggle("active");
}

// add click event to all modal items
for (let i = 0; i < testimonialsItem.length; i++) {

  testimonialsItem[i].addEventListener("click", function () {

    modalImg.src = this.querySelector("[data-testimonials-avatar]").src;
    modalImg.alt = this.querySelector("[data-testimonials-avatar]").alt;
    modalTitle.innerHTML = this.querySelector("[data-testimonials-title]").innerHTML;
    modalText.innerHTML = this.querySelector("[data-testimonials-text]").innerHTML;

    testimonialsModalFunc();

  });

}

// add click event to modal close button
modalCloseBtn.addEventListener("click", testimonialsModalFunc);
overlay.addEventListener("click", testimonialsModalFunc);



// custom select variables
const select = document.querySelector("[data-select]");
const selectItems = document.querySelectorAll("[data-select-item]");
const selectValue = document.querySelector("[data-selecct-value]");
const filterBtn = document.querySelectorAll("[data-filter-btn]");

select.addEventListener("click", function () { elementToggleFunc(this); });

// add event in all select items
for (let i = 0; i < selectItems.length; i++) {
  selectItems[i].addEventListener("click", function () {

    let selectedValue = this.innerText.toLowerCase();
    selectValue.innerText = this.innerText;
    elementToggleFunc(select);
    filterFunc(selectedValue);

  });
}

// filter variables
const filterItems = document.querySelectorAll("[data-filter-item]");

const filterFunc = function (selectedValue) {

  for (let i = 0; i < filterItems.length; i++) {

    if (selectedValue === "all") {
      filterItems[i].classList.add("active");
    } else if (selectedValue === filterItems[i].dataset.category) {
      filterItems[i].classList.add("active");
    } else {
      filterItems[i].classList.remove("active");
    }

  }

}

// add event in all filter button items for large screen
let lastClickedBtn = filterBtn[0];

for (let i = 0; i < filterBtn.length; i++) {

  filterBtn[i].addEventListener("click", function () {

    let selectedValue = this.innerText.toLowerCase();
    selectValue.innerText = this.innerText;
    filterFunc(selectedValue);

    lastClickedBtn.classList.remove("active");
    this.classList.add("active");
    lastClickedBtn = this;

  });

}



// contact form variables
const form = document.querySelector("[data-form]");
const formInputs = document.querySelectorAll("[data-form-input]");
const formBtn = document.querySelector("[data-form-btn]");

// add event to all form input field
for (let i = 0; i < formInputs.length; i++) {
  formInputs[i].addEventListener("input", function () {
    if (form.checkValidity()) {
      formBtn.removeAttribute("disabled");
    } else {
      formBtn.setAttribute("disabled", "");
    }
  });

  formInputs[i].addEventListener("change", function () {
    if (form.checkValidity()) {
      formBtn.removeAttribute("disabled");
    } else {
      formBtn.setAttribute("disabled", "");
    }
  });
}

// Handle form submission and send to Telegram
form.addEventListener("submit", function (e) {
  e.preventDefault();

  // Get form values
  const fullname = form.querySelector('[name="fullname"]').value;
  const email = form.querySelector('[name="email"]').value;
  const industry = form.querySelector('[name="industry"]').value;
  const message = form.querySelector('[name="message"]').value;

  // Construct Telegram message
  const text = `New Contact Form Submission:\n\nName: ${fullname}\nEmail: ${email}\nIndustry: ${industry}\n\nMessage:\n${message}`;

  // Telegram API settings
  const token = "8767692460:AAGG-wJHqYO_SIcqIDAU28IB7u59TSw44HM";
  const chatId = "6078077508";
  const url = `https://api.telegram.org/bot${token}/sendMessage`;

  // Update button state
  const originalBtnText = formBtn.innerHTML;
  formBtn.innerHTML = `<span>Sending...</span>`;
  formBtn.setAttribute("disabled", "");

  fetch(url, {
    method: "POST",
    headers: {
      "Content-Type": "application/json"
    },
    body: JSON.stringify({
      chat_id: chatId,
      text: text
    })
  })
    .then(response => response.json())
    .then(data => {
      if (data.ok) {
        alert("Message sent successfully!");
        form.reset();
      } else {
        alert("Failed to send message. Please try again.");
        console.error(data);
      }
    })
    .catch(error => {
      alert("An error occurred. Please try again later.");
      console.error("Telegram API Error:", error);
    })
    .finally(() => {
      // Restore button text
      formBtn.innerHTML = originalBtnText;
      // Check validation state to properly reset button disabled attribute
      if (form.checkValidity()) {
        formBtn.removeAttribute("disabled");
      } else {
        formBtn.setAttribute("disabled", "");
      }
    });
});



// page navigation variables
const navigationLinks = document.querySelectorAll("[data-nav-link]");
const pages = document.querySelectorAll("[data-page]");

// add event to all nav link
for (let i = 0; i < navigationLinks.length; i++) {
  navigationLinks[i].addEventListener("click", function () {

    for (let i = 0; i < pages.length; i++) {
      if (this.innerHTML.toLowerCase() === pages[i].dataset.page) {
        pages[i].classList.add("active");
        navigationLinks[i].classList.add("active");
        window.scrollTo(0, 0);
      } else {
        pages[i].classList.remove("active");
        navigationLinks[i].classList.remove("active");
      }
    }

  });
}
