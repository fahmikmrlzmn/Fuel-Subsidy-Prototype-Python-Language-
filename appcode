import streamlit as st 
from datetime import datetime

st.title("Fuel Subsidy Prototype (With Vehicle Verification)")
st.write("Fraud detection using distance, time rules, and registered vehicle validation.")

# ---------------------------------------
# Fake citizen & vehicle database
# ---------------------------------------
citizens = {
    "900101081234": {"name": "Ali", "eligible": True},
    "850202061111": {"name": "Siti", "eligible": False},
}

vehicles = {
    "900101081234": ["ABC1234", "VDF9988"],  # Ali owns two vehicles
    "850202061111": ["KLM4455"],             # Siti owns one vehicle
}

# Store transactions
if "transactions" not in st.session_state:
    st.session_state.transactions = []

# ---------------------------------------
# Eligibility Check
# ---------------------------------------
st.header("Check Eligibility")

mykad = st.text_input("Enter MyKad")
vehicle_no = st.text_input("Enter Vehicle Number")

if st.button("Check Eligibility"):
    if mykad in citizens:
        # Eligibility result
        if citizens[mykad]["eligible"]:
            st.success(f"{citizens[mykad]['name']} is eligible.")
        else:
            st.error(f"{citizens[mykad]['name']} is NOT eligible.")

        # Vehicle verification
        if mykad in vehicles and vehicle_no in vehicles[mykad]:
            st.success(f"Vehicle {vehicle_no} is registered under this MyKad.")
        else:
            st.error(f"Vehicle {vehicle_no} is NOT registered under this MyKad.")

    else:
        st.warning("MyKad not found.")

# ---------------------------------------
# Add Transaction
# ---------------------------------------
st.header("Add Fuel Transaction")

t_mykad = st.text_input("MyKad for Transaction")
t_vehicle = st.text_input("Vehicle Number for Transaction")
station = st.text_input("Station Name")
distance_from_prev = st.number_input("Distance from previous station (KM)", value=0.0)

if st.button("Add Transaction"):
    ts = datetime.now()

    # MUST check that both MyKad and Vehicle are valid
    if t_mykad in vehicles and t_vehicle in vehicles[t_mykad]:
        # Now add FULL transaction info including vehicle number
        st.session_state.transactions.append((t_mykad, t_vehicle, station, distance_from_prev, ts))
        st.success("Transaction added!")
    else:
        st.error("Transaction rejected: Vehicle is NOT registered under this MyKad.")

# ---------------------------------------
# Fraud Detection
# ---------------------------------------
st.header("Fraud Detection Rules")

if st.button("Run Fraud Check"):
    flagged = []
    txs = st.session_state.transactions

    for i in range(len(txs)):
        mk1, v1, s1, d1, t1 = txs[i]

        for j in range(i + 1, len(txs)):
            mk2, v2, s2, d2, t2 = txs[j]

            # Compare only same user AND same vehicle
            if mk1 != mk2 or v1 != v2:
                continue

            time_diff_minutes = abs((t2 - t1).total_seconds()) / 60

            # Rule 1: Impossible travel speed
            if d2 > 40 and time_diff_minutes < 15:
                flagged.append(
                    (mk1, v1, "Rule 1: Impossible speed", s1, s2, d2, time_diff_minutes)
                )

            # Rule 2: Too frequent transactions
            if time_diff_minutes < 20:
                flagged.append(
                    (mk1, v1, "Rule 2: Too frequent transactions", s1, s2, time_diff_minutes)
                )

            # Rule 3: Very low distance
            if d2 < 1:
                flagged.append(
                    (mk1, v1, "Rule 3: Distance too small", s1, s2, d2)
                )

    if flagged:
        st.error("Fraud Detected:")
        for f in flagged:
            st.write(f)
    else:
        st.success("No fraud detected.")

# ---------------------------------------
# Show Transactions
# ---------------------------------------
st.header("All Transactions")
st.write(st.session_state.transactions)
