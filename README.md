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


This the code.



<?php

// Hook to initiate interest calculation after order completion
add_action('woocommerce_order_status_completed', 'start_interest_calculation');

function start_interest_calculation($order_id) {
    $completed_time = get_post_meta($order_id, '_completed_date', true);
    $current_time = current_time('timestamp');
    $elapsed_time = $current_time - strtotime($completed_time);

    if ($elapsed_time < 86400) { // 86400 seconds = 24 hours
        $order_amount = get_post_meta($order_id, '_order_total', true);
        $interest = $order_amount * 0.25;

        // Code to add interest amount to user's WooCommerce wallet
        update_user_wallet($order_id, $interest);

        // Schedule event to stop interest calculation after 24 hours
        wp_schedule_single_event(current_time('timestamp') + 86400, 'stop_interest_calculation', array($order_id));
    }
}




// Hook to stop interest calculation after 24 hours
add_action('stop_interest_calculation', 'stop_interest_calculation');

function stop_interest_calculation($order_id) {
    // Code to stop interest calculation for the given order
    update_order_interest_status($order_id);
}




// Hook to restart interest calculation and refund old order on new order placement
add_action('woocommerce_new_order', 'restart_interest_calculation');

function restart_interest_calculation($order_id) {
    $order_status = get_post_status($order_id);

    if ($order_status === 'completed') {
        // Code to refund old order amount along with interest to user's WooCommerce wallet
        refund_old_order($order_id);

        // Restart 24-hour interest calculation for the new order
        start_interest_calculation($order_id);
    }
}

// Hook to handle the 15th day special case
add_action('woocommerce_new_order', 'handle_15th_day_special_case');





/**
 * Function to handle the 15th day special case.
 *
 * @param int $order_id Order ID.
 */
function handle_15th_day_special_case($order_id) {
    $current_day = date('j');

    if ($current_day == 15) {
        // Do not calculate interest or refund on the 15th day
        return;
    }

    // Continue with the regular processing for other days
    // Add any additional logic specific to other days if needed
    start_interest_calculation($order_id);
}





// Functions - Implement these based on your requirements

/**
 * Function to update user's WooCommerce wallet with interest amount.
 *
 * @param int $order_id Order ID.
 * @param float $interest Interest amount to be added to the wallet.
 */
function update_user_wallet($order_id, $interest) {
    // Get the user ID associated with the order
    $user_id = get_post_meta($order_id, '_customer_user', true);




    // Check if a user ID is available
    if ($user_id) {
        // Get the current wallet balance from user meta
        $current_wallet_balance = get_user_meta($user_id, 'wallet_balance', true);

        // If the user meta doesn't exist, initialize the wallet balance
        if (!$current_wallet_balance) {
            $current_wallet_balance = 0.0;
        }

        // Calculate the new wallet balance after adding interest
        $new_wallet_balance = $current_wallet_balance + $interest;

        // Update user meta with the new wallet balance
        update_user_meta($user_id, 'wallet_balance', $new_wallet_balance);
    }
}





/**
 * Function to update order interest status.
 *
 * @param int $order_id Order ID.
 */
function update_order_interest_status($order_id) {
    // Implement code to update order interest status
    // Example: update_post_meta($order_id, '_interest_calculated', true);

    // Adding custom meta field '_interest_calculated' to mark interest as calculated
    update_post_meta($order_id, '_interest_calculated', true);
}





/**
 * Function to refund old order amount along with interest to user's WooCommerce wallet.
 *
 * @param int $order_id Order ID.
 */
function refund_old_order($order_id) {
    // Get the order object
    $order = wc_get_order($order_id);

    // Ensure the order exists
    if ($order) {
        // Calculate the refund amount (order total + interest)
        $refund_amount = $order->get_total() + get_post_meta($order_id, '_order_interest', true);





        // Create a refund for the entire order amount
        $refund = wc_create_refund(array(
            'order_id'     => $order_id,
            'amount'       => $refund_amount,
            'reason'       => 'Interest Refund',
            'refund_id'    => uniqid('refund_'), // Generate a unique refund ID
        ));

        if (is_wp_error($refund)) {
            // Handle error
            error_log('Error creating refund: ' . $refund->get_error_message());



        } else {
            // Update order meta to track that a refund has been processed
            update_post_meta($order_id, '_refund_processed', true);

            // Optionally update other order-related information
            // ...

            // Log refund information
            error_log('Refund created successfully for order ID: ' . $order_id);
        }
    }
}



?>
