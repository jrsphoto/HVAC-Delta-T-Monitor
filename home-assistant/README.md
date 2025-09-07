# Home Assistant Integration - HVAC Delta-T Monitor

This directory contains the Home Assistant configuration files for the HVAC Delta-T monitoring system. These files provide comprehensive dashboard visualization and intelligent automation for HVAC system monitoring and maintenance.

## 📁 Files Overview

### `dashboard.yaml`
**Complete HVAC monitoring dashboard with advanced diagnostics**

The dashboard provides real-time and historical monitoring of your HVAC system performance through multiple visualization methods:

#### **Current Status Monitoring**
- **Delta-T Gauge**: Visual efficiency indicator with color-coded zones (Excellent ≥14°F, Good 10-14°F, Fair 7-10°F, Poor <7°F)
- **Static Pressure Gauge**: Real-time pressure monitoring (0-300 Pa range) with alert thresholds
- **Temperature Display**: Current return and supply air temperatures

#### **Historical Analysis**
- **24-Hour Trends**: Temperature and pressure graphs for daily pattern analysis
- **7-Day Statistics**: Statistical analysis showing mean, min, and max values for trend identification
- **Mini Graphs**: Quick visual overview of all metrics

#### **Smart Diagnostics**
- **HVAC Efficiency Status**: Automated efficiency classification based on Delta-T performance
- **System Pressure Analysis**: Pressure status with normal/elevated/high indicators
- **Filter Health Monitor**: Intelligent filter status combining Delta-T and pressure data
- **Airflow Assessment**: Detects restrictions and vent closure issues

#### **Advanced Features**
- **Pressure Unit Conversion**: Displays pressure in both Pa and inches H2O
- **Expandable Details**: Detailed pressure analysis with status interpretation
- **Responsive Design**: Works on desktop and mobile devices

### `automations.yaml`
**Intelligent automation suite for proactive HVAC maintenance**

The automation system provides multi-tier alerting and maintenance scheduling:

#### **🚨 Critical Alerts (Immediate Action Required)**
- **Filter Replacement Emergency**: Triggers when Delta-T <7°F AND pressure >200Pa for 10+ minutes
- **Extreme Pressure Warning**: Alerts when pressure exceeds 300Pa for 5+ minutes with optional visual indicators

#### **🔧 Preventive Maintenance**
- **Filter Check Reminder**: Early warning when Delta-T drops to 7-10°F with elevated pressure
- **Vent Restriction Alert**: Detects high pressure with good Delta-T (indicates closed vents)
- **Weekly Maintenance Check**: Sunday evening filter status reminder with current performance data

#### **📊 Performance Monitoring**
- **Daily Reports**: Morning summary of system performance trends
- **Seasonal Reminders**: Quarterly maintenance check notifications (Mar, Jun, Sep, Dec)
- **Performance Recovery**: Positive confirmation when system returns to optimal operation

#### **🛡️ System Health**
- **Sensor Monitoring**: Alerts when any sensor goes offline for 5+ minutes
- **Data Logging**: Daily performance summaries for long-term trend analysis

## 🔧 Installation Instructions

### 1. Dashboard Setup
```bash
# Copy dashboard configuration
cp dashboard.yaml /config/dashboards/hvac-monitoring.yaml

# In Home Assistant:
# Settings → Dashboards → Add Dashboard → "From YAML"
# Paste contents of dashboard.yaml
```

### 2. Automations Setup
```bash
# Add to existing automations file
cat automations.yaml >> /config/automations.yaml

# Or create separate file:
cp automations.yaml /config/automations/hvac-monitoring.yaml
```

### 3. Required Customization

#### **Update Notification Services**
Replace placeholder notification services with your actual devices:
```yaml
# Change this:
notify.mobile_app_your_phone

# To your device (examples):
notify.mobile_app_johns_iphone
notify.mobile_app_samsungs_galaxy
notify.email_alerts
```

#### **Optional Light Integration**
For emergency alerts with visual indicators:
```yaml
# Update or remove:
light.living_room_lights  # Replace with your lights
```

## 📱 Dashboard Features

### **Gauge Visualizations**
- **Delta-T Performance**: Real-time efficiency with color coding
- **Static Pressure**: System pressure with alert thresholds

### **Smart Status Cards**
- **Efficiency Indicator**: Excellent/Good/Fair/Poor classification
- **Filter Status**: Combines multiple metrics for accurate filter health
- **Airflow Monitor**: Detects restrictions and vent issues
- **System Pressure**: Normal/Elevated/High status with explanations

### **Historical Data**
- **Trend Analysis**: 24-hour and 7-day performance tracking
- **Statistical Summaries**: Min/max/average calculations
- **Pattern Recognition**: Identify daily and seasonal patterns

## 🤖 Automation Logic

### **Multi-Tier Alert System**
1. **Preventive**: Early warnings for maintenance planning
2. **Critical**: Immediate action required notifications
3. **Emergency**: System protection alerts with escalation

### **Intelligent Diagnostics**
- **Filter Status**: `Low Delta-T + High Pressure = Replace Filter`
- **Airflow Issues**: `High Pressure + Good Delta-T = Check Vents`
- **System Health**: `Combined metrics for accurate diagnosis`

### **Maintenance Scheduling**
- **Daily**: Performance monitoring and trend analysis
- **Weekly**: Filter check reminders based on current performance
- **Seasonal**: Professional maintenance scheduling reminders

## 🎛️ Customization Options

### **Threshold Adjustments**
Modify alert thresholds based on your system's characteristics:
```yaml
# Example threshold modifications in automations:
below: 7     # Change critical Delta-T threshold
above: 200   # Change critical pressure threshold
```

### **Timing Customization**
Adjust notification schedules:
```yaml
# Daily reports
at: "08:00:00"  # Change morning report time

# Weekly reminders
weekday: [sun]  # Change reminder day
```

### **Visual Customization**
Dashboard appearance can be modified:
- **Color schemes**: Modify severity color coding
- **Layout**: Rearrange card positions
- **Card types**: Replace custom cards with built-in alternatives

## 📋 Dependencies

### **Required Custom Cards** (Optional)
Install via HACS for enhanced dashboard features:
- `mushroom-template-card`: Enhanced status displays
- `mini-graph-card`: Compact trend visualization
- `fold-entity-row`: Expandable detail sections

### **Alternative Setup**
The dashboard will work with built-in cards, but some advanced features will display as "Custom element doesn't exist" if custom cards aren't installed.

## 🔍 Troubleshooting

### **Entity Name Verification**
Ensure your ESPHome device creates entities with the expected naming pattern:
```
sensor.delta_tango_192b39_hvac_*
```

### **Notification Testing**
Test notification delivery:
1. Use Developer Tools → Services
2. Call your notification service manually
3. Verify mobile app notification permissions

### **Dashboard Errors**
- Check entity names match exactly
- Verify all sensors are available
- Install required custom cards or use built-in alternatives

## 📈 Performance Monitoring

### **Normal Operating Ranges**
- **Delta-T**: 14-20°F (cooling), 30-40°F (heating)
- **Static Pressure**: 50-125 Pa (0.2-0.5" H2O)
- **Filter Change**: When Delta-T <10°F or pressure >150Pa

### **Alert Escalation**
1. **125-200 Pa**: Elevated pressure monitoring
2. **200+ Pa**: High pressure alerts
3. **300+ Pa**: Emergency system protection

---

**⚠️ Important**: Test all automations carefully and adjust thresholds based on your specific HVAC system's normal operating characteristics. Monitor initial performance to fine-tune alert sensitivity.
