document.addEventListener('DOMContentLoaded', () => {
    // DOM Element References
    const modal = document.getElementById('fullScreenModal');
    const closeModalBtn = document.getElementById('closeModalBtn');
    const modalTitle = document.getElementById('modalTitle');
    const modalText = document.getElementById('modalText');
    const modalSymbol = document.getElementById('modalSymbol');
    const modalContentArea = document.getElementById('modalContentArea');
    const cards = document.querySelectorAll('.card-effect');
    
    // Elements to fade out when modal is open
    const fadeElements = document.querySelectorAll('.content-container-fade');

    // Animation timing variables (Must match CSS transitions for smooth sync)
    const titleAnimationTime = 500; // 0.5s
    const contentBaseDelay = 200; // Delay after title settles before content starts
    const textStaggerTime = 100; // Delay between each line/paragraph fade-in

    /**
     * Opens the full-screen modal with content populated from the clicked card.
     * @param {string} title - The main title for the header.
     * @param {string} text - The detailed explanation text.
     * @param {string} symbolContent - The HTML/text for the visual symbol.
     * @param {string} symbolType - 'text' or 'svg'.
     */
    const openModal = (title, text, symbolContent, symbolType) => {
        
        // 1. Start fading out background content
        fadeElements.forEach(el => el.classList.add('content-faded'));
        
        // 2. Clear content and prepare for title expansion
        modalTitle.textContent = title;
        modalContentArea.classList.add('content-hidden');
        modalText.innerHTML = ''; // Clear previous text content
        
        // 3. Open modal wrapper and disable body scroll
        modal.classList.add('is-open');
        document.body.style.overflow = 'hidden';

        // 4. Content population and staggered fade-in
        setTimeout(() => {
            
            // Populate Text Content (line-by-line fade)
            // Split text into potential sentences/paragraphs for line-by-line effect
            // We split by ". " and re-add the period later to ensure text segments make sense
            const sentences = text.split('. ').map(s => s.trim()).filter(s => s.length > 0);
            
            sentences.forEach((sentence, index) => {
                const p = document.createElement('p');
                // Re-add the period that was split off (or a default period)
                p.textContent = sentence + (text.endsWith('.') && index === sentences.length - 1 ? '' : '.'); 
                
                // Set initial opacity to 0 and apply transition classes
                p.className = 'opacity-0 transition-opacity duration-500 ease-in mb-4 text-lg leading-relaxed text-pure-white/80';
                modalText.appendChild(p);

                // Calculate staggered delay for line-by-line effect
                const staggerDelay = index * textStaggerTime; 

                // Animate the line's opacity
                setTimeout(() => {
                    p.classList.remove('opacity-0');
                }, contentBaseDelay + staggerDelay); // Start after title settles + base delay + stagger
            });
            
            // Populate Symbol (size increased for full screen)
            modalSymbol.innerHTML = '';
            if (symbolType === 'text') {
                modalSymbol.classList.add('font-mono', 'text-[10rem]', 'font-bold');
                modalSymbol.textContent = symbolContent;
            } else if (symbolType === 'svg') {
                modalSymbol.classList.remove('font-mono', 'text-[10rem]', 'font-bold');
                // Increased SVG size for better visual presence
                const svg = `<svg xmlns="http://www.w3.org/2000/svg" width="128" height="128" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" class="mx-auto">${symbolContent}</svg>`;
                modalSymbol.innerHTML = svg;
            }
            
            // Trigger overall content fade-in
            modalContentArea.classList.remove('content-hidden');

        }, titleAnimationTime); // Initial delay to wait for the title animation to complete
    };

    /**
     * Closes the full-screen modal.
     */
    const closeModal = () => {
        // 1. Hide modal content immediately
        modalContentArea.classList.add('content-hidden');

        // 2. Start modal collapse (title shrinks)
        modal.classList.remove('is-open');
        
        // 3. Restore background content and body scroll after transition
        setTimeout(() => {
            document.body.style.overflow = '';
            modal.scrollTop = 0; // Reset scroll position for next open

            // Fade in card grids and other content
            fadeElements.forEach(el => el.classList.remove('content-faded'));
        }, titleAnimationTime); 
    };

    // --- Event Listeners ---
    
    // Listener for all cards to open the modal
    cards.forEach(card => {
        card.addEventListener('click', () => {
            const title = card.getAttribute('data-title');
            const text = card.getAttribute('data-text');
            const symbolContent = card.getAttribute('data-symbol-content');
            const symbolType = card.getAttribute('data-symbol-type');
            openModal(title, text, symbolContent, symbolType);
        });
    });

    // Event listeners for closing the modal
    closeModalBtn.addEventListener('click', closeModal);
    modal.addEventListener('click', (e) => {
        // Close if clicking outside the modal content wrapper
        if (e.target === modal) {
            closeModal();
        }
    });
    document.addEventListener('keydown', (e) => {
        // Close on Escape key press
        if (e.key === 'Escape' && modal.classList.contains('is-open')) {
            closeModal();
        }
    });
});
