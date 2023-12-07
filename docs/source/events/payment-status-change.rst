Payment status change
=====================

This event means that the status of a payment has changed. To reduce noise, we only include status changes that occur after the user interaction has finished and are linked to a form submission. Status changes may also include additional information from the payment provider (e.g., transaction IDs). This usually means that the payment has either succeeded or failed. Note that after a payment fails, the user might choose to retry, which will yield more status changes for the same payment.

For each payment method, there are typical status sequences that the payment goes through. The exact meaning of the statuses and content of the ``payment_data`` also depends on the payment method.

.. literalinclude:: examples/payment-status-change.yaml
  :language: yaml
