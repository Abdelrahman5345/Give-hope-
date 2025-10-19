$(document).ready(function() {
    let animationStarted = false;
    
    // Define the donation categories with their positions
    const donationCategories = [
        { name: "Food", class: "food", angle: 0, icon: "🍲" },
        { name: "Water", class: "water", angle: 60, icon: "💧" },
        { name: "Shelter", class: "shelter", angle: 120, icon: "🏠" },
        { name: "Health", class: "health", angle: 180, icon: "🏥" },
        { name: "Education", class: "education", angle: 240, icon: "📚" },
        { name: "Clothing", class: "clothing", angle: 300, icon: "👕" }
    ];
    
    // Create hand elements
    donationCategories.forEach(category => {
        const hand = $('<div class="hand ' + category.class + '">' + 
                      category.icon + '<span>' + category.name + '</span></div>');
        $('#handsContainer').append(hand);
    });
    
    // Function to show main website
    function showMainWebsite() {
        $('#intro-section').hide();
        $('#main-content').show();
    }
    
    // Heart click animation
    $('#heart').click(function() {
        if (animationStarted) return;
        animationStarted = true;
        
        const $heart = $(this);
        const $handsContainer = $('#handsContainer');
        const $hands = $('.hand');
        
        // Expand animation
        $heart.css({
            'width': '200px',
            'height': '200px',
            'transform': 'translate(-50%, -50%) rotate(-45deg)',
            'animation': 'none'
        });
        
        $handsContainer.css({
            'width': '400px',
            'height': '400px',
            'opacity': '1'
        });
        
        // Position hands in a circle
        const radius = 150;
        $hands.each(function(index) {
            const angle = donationCategories[index].angle * (Math.PI / 180);
            const x = radius * Math.cos(angle);
            const y = radius * Math.sin(angle);
            
            $(this).css({
                'left': `calc(50% + ${x}px)`,
                'top': `calc(50% + ${y}px)`,
                'transform': 'translate(-50%, -50%) scale(1)',
                'transition-delay': `${index * 0.1}s`
            });
        });
        
        $heart.addClass('expanded');
        $('.heart-text').text('Thank You!');
        
        // Show main website after animation completes
        setTimeout(showMainWebsite, 1500);
    });
    
    // Skip button functionality - goes directly to website
    $('#skipBtn').click(function() {
        showMainWebsite();
    });
    
    // Auto-start animation after 3 seconds if user doesn't click
    setTimeout(() => {
        if (!animationStarted) {
            $('#heart').click();
        }
    }, 3000);
    
    // Main website functionality
    // Donation amount selection
    $('.donation-amount').click(function() {
        $('.donation-amount').removeClass('active');
        $(this).addClass('active');
        
        if ($(this).data('amount') === 'custom') {
            $('.custom-amount').show();
            $('#customAmount').focus();
        } else {
            $('.custom-amount').hide();
        }
    });
    
    // Payment method selection
    $('input[name="paymentMethod"]').change(function() {
        var method = $(this).val();
        $('.payment-method').removeClass('active');
        $('#' + method + 'Payment').addClass('active');
    });

    // Form submission
    $('#donationForm').submit(function(e) {
        e.preventDefault();
        
        var amount = 0;
        var activeAmount = $('.donation-amount.active');
        
        if (activeAmount.data('amount') === 'custom') {
            amount = $('#customAmount').val();
        } else {
            amount = activeAmount.data('amount');
        }
        
        if (!amount || amount <= 0) {
            alert('Please select a valid donation amount.');
            return;
        }
        
        alert('Thank you for your donation of $' + amount + '! Your support is making a difference.');
        $(this).trigger('reset');
        $('.donation-amount').removeClass('active');
        $('.donation-amount[data-amount="25"]').addClass('active');
        $('.custom-amount').hide();
        $('.payment-method').removeClass('active');
        $('#cardPayment').addClass('active');
        $('input[name="paymentMethod"][value="card"]').prop('checked', true);
    });
    
    // Sponsors carousel auto-advance
    const sponsorsCarousel = new bootstrap.Carousel('#sponsorsCarousel', {
        interval: 4000, // 4 seconds
        wrap: true,
        pause: false
    });

    // Optional: Pause on hover
    $('#sponsorsCarousel').hover(
        function() {
            sponsorsCarousel.pause();
        },
        function() {
            sponsorsCarousel.cycle();
        }
    );
    
    // Smooth scrolling
    $('a[href^="#"]').on('click', function(e) {
        e.preventDefault();
        
        var target = this.hash;
        var $target = $(target);
        
        $('html, body').animate({
            'scrollTop': $target.offset().top - 80
        }, 800, 'swing');
    });
});

// Category rotation
document.addEventListener("DOMContentLoaded", function () {
    const categories = document.querySelectorAll('.cause-category');
    let currentIndex = 0;

    // Hide all except the first one
    categories.forEach((cat, index) => {
        if (index !== 0) cat.style.display = 'none';
    });

    setInterval(() => {
        // Hide current
        categories[currentIndex].style.display = 'none';

        // Increment index
        currentIndex = (currentIndex + 1) % categories.length;

        // Show next
        categories[currentIndex].style.display = 'block';
    }, 4000); // 4000ms = 4 seconds
});