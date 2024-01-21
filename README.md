# wordpress-issue

I am currently engaged in a WordPress project focused on cryptocurrency trading, and while the primary development phase is complete, I am encountering challenges in implementing necessary automations. To address this, I am utilizing code snippets to execute custom scripts. Unfortunately, the code I am attempting to execute is not functioning as intended, and I am seeking assistance to rectify this issue. Any guidance or support in resolving these challenges would be greatly appreciated.

This are the plugins i used for this project

1. Checkout Field Editor for WooCommerce

2. Code Snippets

3. Cryptocurrency Widgets

4. Direct checkout, WooCommerce Single page checkout , WooCommerce One page checkout

5. Easy Accordion

6. Elementor

7. Fluent Support

8. LiteSpeed Cache

9. Login/Signup Popup

10. MouseWheel Smooth Scroll

11. NotificationX

12. Testimonial Slider and Showcase

13. Ultimate Member

14. Visibility Logic for Elementor

15. Wallet System For WooCommerce

16. WooCommerce

17. Woocommerce Support System

18. WP Force SSL

19. WPForms Lite

20. YITH WooCommerce Affiliates


This the code am trying to execute and it not workig, it a php code.
this what i want the code to do.

1. When a user places a product order in the woocommerce store and the status shows completed, initiate a 24-hour interest calculation of 25% on the order amount.

2. After 24 hours, stop the interest calculation.

3. When the user places a new product order with a completed status, restart the 24-hour interest calculation on the new order. Refund the old order amount along with the interest to the user's WooCommerce wallet.

4. Repeat this process for 15 days.

5. On the 15th day, if a new product order is placed, do not calculate interest and refrain from refunding.

6. Start the process anew on each 15th day.

This ensures a cyclical process with interest calculation and refunding for 15 days, with a special case on the 15th day where interest isn't calculated on the last new order.



