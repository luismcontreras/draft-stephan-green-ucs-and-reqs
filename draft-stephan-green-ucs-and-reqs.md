---
title: "Requirements for Energy Efficiency Management"
abbrev: "Requirements for Energy Efficiency"
docname: draft-stephan-green-ucs-and-reqs-latest
category: info
stand_alone: true

submissiontype: IETF  # also: "independent", "editorial", "IAB", or "IRTF"
consensus: true

area: "Operations and Management"
workgroup: "Getting Ready for Energy-Efficient Networking"
keyword: Sustainability, Ressources

venue:
  group: "Getting Ready for Energy-Efficient Networking"
  type: "Working Group"
  mail: "green@ietf.org"
  arch: "https://mailarchive.ietf.org/arch/browse/green/"
  github: "emile22/draft-stephan-green-ucs-and-reqs"
  latest: "https://emile22.github.io/draft-stephan-green-ucs-and-reqs/draft-stephan-green-ucs-and-reqs.html"

v: 3

author:

  -
    fullname: Emile Stephan
    org: Orange
    email: emile.stephan@orange.com
  -
    fullname: Marisol Palmero
    org: Cisco Systems, Inc.
    email: mpalmero@cisco.com
  -
    fullname: Benoit Claise
    org: Huawei
    email: benoit.claise@huawei.com
  -
    fullname: Qin Wu
    org: Huawei
    email: bill.wu@huawei.com
  -
    fullname: Luis M. Contreras
    org: Telefonica
    email: luismiguel.contrerasmurillo@telefonica.com

normative:

informative:

  operators-inputs:
    title: "Input from Operators to GREEN BoF"
    date: 2024-07-20
    target: "https://datatracker.ietf.org/meeting/120/materials/slides-120-green-input-from-operators-to-green-bof-01"

  ITU-M3400:
    title: "International Telecommunication Union, ITU-T Recommendation M.3400, Series M"
    date: 2000-02-01

  IEC.60050:
    title: "International Electrotechnical Commission, Electropedia: The World's Online Electrotechnical Vocabulary"
    date: 2013
    target: http://www.electropedia.org/iev/iev.nsf/welcome?openform

--- abstract

This document delineates the requirements for standards specifications in Energy Efficiency Management, extending the foundational work of RFC6988 and incorporating recent insights from operator requirements and the GREEN BoF discussions. Eleven years after the publication of RFC6988, this document reassesses and updates the requirements to align with contemporary needs.

The primary objectives of this draft, which are listed in the goals and scope with the creation of the GREEN WG charter, is focusing on two main targets:
(1) collecting and updating requirements for the management of energy-efficient networks, and
(2) defining use cases for managing energy-efficient networks.

Requirements are segmented into three core functions: discovery, monitoring, and control. Discovery functions involve identifying energy-managed networks, devices, and their components, as well as discovering the inventory of power components capabilities, optimization control capabilities, and nominal condition use. Monitoring functions encompass tracking power states, power attributes, energy consumption, network performance, and energy efficiency metrics. Control functions include managing energy-saving and optimization functions and the power states of energy-managed devices and their components.

This document does not mandate specific use cases for compliant implementations but rather lists a few use cases that must be supported by standards for Energy Efficiency Management.

Terms and definitions related to energy efficiency metrics will be discussed in later stages for potential integration.

This draft merges two drafts: (draft-eman-green-rfc6988bis-01)[https://datatracker.ietf.org/doc/draft-eman-green-rfc6988bis/] and (draft-stephan-green-bof-reqs-collections-02)[https://datatracker.ietf.org/doc/draft-stephan-green-bof-reqs-collections/]

Discussion Venues

Source of this draft and an issue tracker can be found at https://github.com/emile22/draft-stephan-green-terms-ucs-and-reqs


--- middle

# Introduction

At this step the specifications made by the IETF, aka in WGs like EMAN, on energy managements focus mainly on SMI (aka MIBs) instead of YANG and cover neither the control nor energy efficiency.
As a consequence, the willing of this document is to resuse pieces of the energy-related requirements of RFC6988 and to map them in a framework of YANG/Netconf for energy efficiency that might reuse "YANG Data Model for Hardware Management" {{?RFC8348}}, a conversion of former Entity MIB module, Entity Sensor MIB module, Entity State MIB modules.

The following section will delve into the specific details but from a high level point of view, the differences between this document and the RFC6988 are:

- New definition for "Energy Efficiency Management"

- A focus towards YANG, and not any longer on MIB modules

- Usage of the Hardware YANG module RFC8348

- A focus on reporting lifecycle management, considering energy and transformation towards carbon awareness

However, Power and Energy Monitoring and Control MIB modules has not been converted yet into YANG modules.

## Background

   With rising energy costs and an increasing awareness of the
   environmental impact of running information technology equipment, Energy
   efficiency Management functions and management interfaces are becoming
   an additional basic requirement for network management systems and devices
   connected to a network.

   This document defines requirements for standards specifications for
   Energy efficiency Management, including discovery functions, monitoring functions
   and control functions.
   Energy efficiency Management functions focus mainly on network devices and
   their built-in components that receive and provide electrical energy.  Devices such
   as switches, routers, servers and storage devices should have an IP address providing a
   management interface for the network device. Alternatively, energy-related devices (for
   example, in building management, which typically don't support IP) might be connected
   via a proxy/gateway with an IP address.

   These requirements are concerned with the standards specification
   process and not the implementation of specified standards.  All
   requirements in this document must be reflected by standards
   specifications to be developed.  However, which of the features
   specified by these standards will be mandatory, recommended, or
   optional for compliant implementations is to be defined by Standards
   Track document(s) and not in this document.

   Section 3 elaborates on a set of general needs for Energy Management.
   Requirements for an Energy Management standard are specified in
   Sections 4 through 8.

   Sections 4 through 6 contain conventional requirements specifying
   information on entities and control functions.

   Sections 7 and 8 contain requirements specific to Energy Management.
   Due to the nature of power supply, some monitoring and control
   functions are not conducted by interacting with the entity of
   interest but rather with other entities, for example, entities
   upstream in a power distribution tree.

## Conventional Requirements for Energy Efficiency Management

   The specification of requirements for an Energy Efficiency Management standard
   starts with Section 4, which addresses the identification of entities
   and the granularity of reporting of energy-related information.  A
   standard must support the unique identification of entities,
   reporting per network, per entire device, and reporting energy-related information
   on individual components of a device or attached devices.

   Section 5 specifies requirements related to the monitoring of
   entities.  This includes general (type, context) information and
   specific information on Power States, Power Inlets, Power Outlets,
   power, energy.  The control of Power State and power saving functionalities,
  optimization functionalities by entities is covered by requirements specified in Section 6.

## Specific Requirements for Energy Management

   While the conventional requirements summarized above seem to be all
   that would be needed for Energy Management, there are significant
   differences between Energy Management and most well-known network
   management functions.  The most significant difference is the need
   for some devices to report on other entities.  There are two major
   reasons for this.

   o  For monitoring a particular entity, it is not always sufficient to
      communicate only with that entity.  When the entity has no
      instrumentation for determining power, it might still be possible
      to obtain power values for the entity via communication with other
      entities in its power distribution tree.  A simple example of this
      would be the retrieval of power values from a power meter at the
      power line into the entity.  A Power Distribution Unit (PDU) and a
      Power over Ethernet (PoE) switch are common examples.  Both supply
      power to other entities at sockets or ports, respectively, and are
      often instrumented to measure power per socket or port. Also it
      could be considered to obtain power values for the entity via
      communication with other entities outside of the power distribution
      tree, like for example external databases or even data sheets.

   o  Similar considerations apply to controlling the power supply of an
      entity that often needs direct or indirect communications with
      another entity upstream in the power distribution tree.  Again, a
      PDU and a PoE switch are common examples, if they have the
      capability to switch power on or off at their sockets or ports,
      respectively.

   These specific issues of Energy Management, as well as other issues,
   are covered by requirements specified in Sections 7 and 8.

   The requirements in these sections need a new Energy Management
   framework that deals with the specific nature of Energy Management.
   The actual standards documents, such as MIB module specifications,
   address conformance by specifying which features must, should, or may
   be implemented by compliant implementations.

# Terminology

The terms below are a sub set of the whole terminology. There are many other drafts givving additionnal definitons.

   The terms specified in the terminology section are capitalized
   throughout the document; the exceptions are the well-known terms
   "energy" and "power".  These terms are generic and are used in
   generated terms such as "energy-saving", "low-power", etc.

   Embedded carbon (or embodied carbon)

      The total amount of greenhouse gas emissions, measured in tonnes
      of CO2 equivalent (tCO2e), associated with the entire lifecycle
      of a product or material, from raw material extraction through
      manufacturing, transportation, use, and end-of-life disposal or
      recycling.

   Embodied energy

      The total amount of energy consumed in all processes associated
      with the production of a building material or product, from the
      extraction and processing of raw materials, through manufacturing,
      transportation, and installation, to the end of its useful life,
      including disposal or recycling.

   Energy

      Energy is the capacity of a system to do work.  As used by
      electric utilities, it is generally a reference to electrical
      energy and is measured in kilowatt-hours (kWh) [IEEE-100].

   Power

      Power is the time rate at which energy is emitted, transferred, or
      received; power is usually expressed in watts (or in joules per
      second) [IEEE-100].  (The term "power" does not refer to the
      concept of demand, which is an averaged power value.)

   Power Attributes

      Power Attributes are measurements of electric current, voltage,
      phase, and frequencies at a given point in an electrical power
      system (adapted from [IEC.60050]).

      NOTE: Power Attributes are not intended to be "judgmental" with
      respect to a reference or technical value and are independent of
      any usage context.

   Energy Management

      Energy Management is a set of functions for measuring, modeling,
      planning, and optimizing networks to ensure that the network
      elements and attached devices use energy efficiently and in a
      manner appropriate to the nature of the application and the cost
      constraints of the organization [ITU-M3400].

   Energy Efficiency Management

     Involves deploying and managing network infrastructures with the
     goal of optimizing energy use on network devices while improving
     the overall network utilization.

   Energy Management System

      An Energy Management System is a combination of hardware and
      software used to administer a network with the primary purpose
      being Energy Management.

   Energy Monitoring

      Energy Monitoring is a part of Energy Efficiency Management that deals
      with collecting or reading information from network elements and their
      components to aid in Energy Efficiency Management.

   Energy Control

      Energy Control is a part of Energy Management that deals with
      controlling energy supply and Power State of network elements, as
      well as their components.

   Power Interface

      A Power Interface is an interface at which a device is connected
      to a power transmission medium, at which it can in turn receive
      power, provide power, or both.

   Power Inlet

      A Power Inlet is a Power Interface at which a device can receive
      power from other devices.

   Power Outlet

      A Power Outlet is a Power Interface at which a device can provide
      power to other devices.

   Power State

      A Power State is a condition or mode of a device that broadly
      characterizes its capabilities, power consumption, and
      responsiveness to input [IEEE-1621].

# General Considerations Related to Energy Management

   The basic objective of Energy Efficiency Management is to operate sets of
   network devices using minimal energy, while maintaining a certain level of
   service.

## Power States

   Entities can be set to an operational state that results in the
   lowest power level that still meets the service-level performance
   objectives.  In principle, there are three basic types of Power
   States for an entity or for a whole system:

   o  full Power State

   o  sleep state (not functional but immediately available)

   o  off state (may require significant time to become operational)

   In specific network devices, the number of Power States and their properties
   vary considerably.  Simple entities may only have the extreme states:
   full Power State and off state.  Many network devices have three basic Power
   States: on, off, and sleep.  However, more finely grained Power
   States can be implemented, especially when Energy efficiency gains for communication
   systems are highly sought after, for environmental, business, and technical reasons.
   Examples are various operational low Power States in which a network device requires less energy than in the full
   power "on" state, but -- compared to the sleep state -- is still
   operational with reduced performance or functionality.

   Another example is standby power state
   in which network device has multiple standby components and one active component for the same functionality,
   standby components are partially functional and can be immediately available when active component is down.
   The standby power state can be introduced to save energy while impove the overall network utilization.

## Saving Energy versus Maintaining Service Level

   One of the objectives of Energy Efficiency Management is to reduce energy
   consumption.  While this objective is clear, attaining that goal is
   often difficult.  In many cases, there is no way to reduce power
   without the consequence of a potential service (performance or
   capacity) degradation.  In this case, a trade-off needs to be made
   between service-level objectives (e.g., network performance) and
   energy minimization. In other cases, a reduction of power can easily be achieved
   while still maintaining sufficient service-level performance, for example, by
   switching entities to lower Power States when higher performance is
   not needed. To measure of the trade-off between service-level object and energy
   consumption, a new set of energy efficiency metrics needs to defined.

## Local versus Network-Wide Energy Management

   Many energy-saving functions are executed locally by an entity; it
   monitors its usage and dynamically adapts its power according to the
   required performance.  It may, for example, switch to a sleep state
   or backup state when it is not in use, or outside of scheduled business hours. An
   Energy Efficiency Management System may observe an entity's Power State and
   configure or optimize its power-saving policies.

   Energy savings can also be achieved with policies implemented by a
   network management system that controls Power States of managed
   entities.  Information about the power received and provided by
   entities in different Power States may be required in order to set
   such policies.  Often, this information is best acquired through
   monitoring.

   Network-wide and local Energy Management methods both have advantages
   and disadvantages, and it is often desirable to combine them.
   Central management is often favorable for setting Power States of a
   large number of entities at the same time, for example, at the
   beginning and end of business hours in a building.  Local management
   is often preferable for power-saving measures based on local
   observations, such as the high or low functional load of an entity.

## Energy Monitoring versus Energy Saving

   Monitoring energy, power, and Power States alone does not reduce the
   energy needed to run an entity.  In fact, it may even increase it
   slightly due to monitoring instrumentation that needs energy.
   Reporting measured quantities over the network may also increase
   energy use, though the acquired information may be an essential input
   to control loops that save energy.

   Monitoring energy and Power States can also be required for other
   purposes, including:

   o  investigating energy-saving potential

   o  evaluating the effectiveness of energy-saving policies and
      measures

   o  deriving, implementing, and testing power management strategies

   o  accounting for the total power received and provided by an entity,
      a network, or a service

   o  predicting an entity's reliability based on power usage

   o  choosing the time of the next maintenance cycle for an entity



## Overview of Energy Management Requirements

   The following basic management functions are required:

   o  monitoring Power States

   o  monitoring power (energy conversion rate)

   o  monitoring (accumulated) received and provided energy

   o  monitoring Power Attributes

   o  setting Power States

   In addition, to support energy efficiency management, additional requirements concerned with discovery functions
   and control functions are introduced:

   o   discovering energy-managed network, devices and their components

   o   discovering inventory of power components together with their capabilities, optimization
        control capabilities, nominal condition use

   o  discovering supported power state of each network device within the network

   o discovering power relationship between component within network device and across network devices.

   o  support additional energy efficiency metrics for energy efficiency monitoring, e.g., heat consumption, energy
       efficiency ratio, maximum wake up time, etc.

   o  support separation of desired power state and actual power state and optimize energy usage to allow update actual
       power state to match desired power state.

  o  Introduce energy saving method, and energy efficiency metrics to support explicit power control or energy
      efficiency optimization and control.

  o  allow control and optimize energy usage to make the trade-off between network performance and power
     consumption.

  o support both local management and network wide management based on energy saving
     functionality.

   Energy usage control and optimization is complementary to other energy-saving design, such
   as low-power electronics,  energy-efficient device design (for example, low-power modes for components), and
   energy-efficient network architectures and is exercised using management interface.  Measurement of received and
   provided energy can provide useful data for energy efficiency management.

# Identification of Entities

   Entities must be capable of being uniquely identified within the
   context of the management system.  This includes entities that are
   components of managed devices as well as entire devices or the entire network.

   Entities that report on or control other entities must identify the
   entities they report on or control: see Section 7 or Section 8,
   respectively, for the detailed requirements.

   An entity may be an entire network, or network device or a component of it.  Examples of
   components of interest are a hard drive, a fan, or a line card.
   The ability to control individual components to save energy may be
   required.  For example, server blades can be switched off when the
   overall load is low, or line cards at switches may be powered down at
   night.

   Identifiers for network, network devices and components are already defined in
   standard YANG modules Network Topology YANG module {{?RFC8345}} and Hardware YANG
   module {{?RFC8348}}. Note that Network Topology YANG module {{?RFC8345}} identifiers
   are reused in the Network Inventory YANG module  {{?I-D.ietf-ivy-network-inventory-yang}}
   and are also the basis for the Digital Map Modeling efforts in the NMOP Working Group.

   Instrumentation for measuring the received and provided energy of a
   device is typically more expensive than instrumentation for
   retrieving its Power State.  Many devices may provide Power State
   information for all individual components separately, while reporting
   the received and provided energy only for the entire device.

## Identifying Entities

   The standard must provide means for uniquely identifying entities.
   Uniqueness must be preserved such that collisions of identities are
   avoided at potential receivers of monitored information.

## Identifying Entitiy Capabilities

   The standard must provide means for discovering inventory of power components
   together with their capabilities, optimization control capabilities, nominal
   condition use. In addition, The standard must provide means for discovering
   supported power state of each network device within the network and power
   relationship between component within network device and across network devices.

## Persistence of Identifiers

   The standard must provide means for indicating whether identifiers of
   entities are persistent across a restart of the entity.

## Change of Identifiers

   The standard must provide means to indicate any change of entity
   identifiers.

## Using Entity Identifiers of Existing YANG Modules

   The standard must provide means for reusing entity identifiers from
   existing standards, including at least the following:

   o  the network-id, link-id, node-id, port-id of the Network Topology YANG module {{?RFC8345}}

   o  the ne-id, Universal Unique IDentifier (UUID) of the network element and component-id, UUID of each component within the network element in Network Inventory YANG module {{?I-D.ietf-ivy-network-inventory-yang}}

   o  the name, UUID of each hardware component in the Hardware YANG module {{?RFC8348}}


   Generic means for reusing other entity identifiers must be provided.

# Information on Entities

   This section describes information on entities for which the standard
   must provide means for retrieving and reporting.

   Required information can be structured into seven groups.
   Section 5.1 specifies requirements for general information on
   entities, such as type of entity or context information.
   Requirements for information on Power Inlets and Power Outlets of
   entities are specified in Section 5.2.  The monitoring of power and
   energy is covered by Sections 5.3 and 5.5, respectively.  Section 5.4
   covers requirements related to entities' Power States.  Finally, the
   reporting of time series of values is covered by Section 5.7.

## General Information on Entities

   For Energy Management, understanding the role and context of an
   entity may be required.  An Energy Management System may aggregate
   values of received and provided energy according to a defined
   grouping of entities.  When controlling and setting Power States, it
   may be helpful to understand the grouping of the entity and role of
   an entity in a network.  For example, it may be important to exclude
   some mission-critical network devices from being switched to lower
   power or even from being switched off.

### Type of Entity

   The standard must provide means to configure, retrieve, and report a
   textual name or a description of an entity.

### Context of an Entity

   The standard must provide means for retrieving and reporting context
   information on entities, for example, tags associated with an entity
   that indicate the entity's role.

### Significance of Entities

   The standard must provide means for retrieving and reporting the
   significance of entities within its context, for example, how
   important the entity is.

### Power Priority

   The standard must provide means for retrieving and reporting power
   priorities of entities.  Power priorities indicate an order in which
   Power States of entities are changed, for example, to lower Power
   States for saving power.

### Grouping of Entities

   The standard must provide means for grouping entities.  This can be
   achieved in multiple ways, for example, by providing means to tag
   entities, assign them to domains, or assign device types to them.

## Power Interfaces

   A Power Interface is an interface at which a device is connected to a
   power transmission medium, at which it can in turn receive power,
   provide power, or both.

   A Power Interface is either an inlet or an outlet.  Some Power
   Interfaces change over time from being an inlet to being an outlet
   and vice versa.  However, most Power Interfaces never change.

   Network Devices have Power Inlets at which they are supplied with electric
   power.  Most devices have a single Power Inlet, while some have
   multiple inlets.  Different Power Inlets on a device are often
   connected to separate power distribution trees.  For Energy
   Monitoring, it is useful to retrieve information on the number of
   inlets of a device, the availability of power at inlets, and which
   inlets are actually in use.

   Network Devices can have one or more Power Outlets for supplying other
   devices with electric power.

   For identifying and potentially controlling the source of power
   received at an inlet, identifying the Power Outlet of another network device
   at which the received power is provided may be required.
   Analogously, for each outlet, it is of interest to identify the Power
   Inlets that receive the power provided at a certain outlet.  Such
   information is also required for constructing the wiring topology of
   electrical power distribution to devices.

   Static properties of each Power Interface are required information
   for Energy Efficiency Management.  Static properties include the kind of
   electric current (AC or DC), the nominal voltage, the nominal AC
   frequency, and the number of AC phases.  Note that the nominal
   voltage is often not a single value but a voltage range, such as, for
   example, (100V-120V), (100V-240V), (100V-120V,220V-240V).

### List of Power Interfaces

   The standard must provide means for monitoring the list of Power Interfaces of a device.

### Operational Mode of Power Interfaces

   The standard must provide means for monitoring the operational mode of a Power Interface, which is either "Power Inlet" or "Power Outlet".

### Corresponding Power Outlet

   The standard must provide means for identifying the Power Outlet that
   provides the power received at a Power Inlet.

### Corresponding Power Inlets

   The standard must provide means for identifying the list of Power
   Inlets that receive the power provided at a Power Outlet.

### Availability of Power

   If the Power States allow it, the standard must provide means for
   monitoring the availability of power at each Power Interface.  This
   includes indicating whether a power supply at a Power Interface is
   switched on or off.

### Use of Power

   The standard must provide means for monitoring each Power Interface
   if it is actually in use.  For inlets, this means that the device
   actually receives power at the inlet.  For outlets, this means that
   power is actually provided from the outlet to one or more devices.

### Type of Current

   The standard must provide means for reporting the type of current (AC
   or DC) for each Power Interface as well as for a device.

### Nominal Voltage Range

   The standard must provide means for reporting the nominal voltage
   range for each Power Interface.

### Nominal AC Frequency

   The standard must provide means for reporting the nominal AC
   frequency for each Power Interface.

### Number of AC Phases

   The standard must provide means for reporting the number of AC phases
   for each Power Interface.

## Power

   Power is measured as an instantaneous value or as the average over a
   time interval.

   Obtaining highly accurate values for power and energy may be costly
   if dedicated metering hardware is required.  Entities without the
   ability to measure with high accuracy their power, received energy,
   and provided energy may just report estimated values, for example,
   based on load monitoring, Power State, or even just the entity type.

   Depending on how power and energy values are obtained, the confidence
   in a reported value and its accuracy will vary.  Entities reporting
   such values should qualify the confidence in the reported values and
   quantify the accuracy of measurements.  For reporting accuracy, the
   accuracy classes specified in IEC 62053-21 [IEC.62053-21] and
   IEC 62053-22 [IEC.62053-22] should be considered.

   Further properties of the power supplied to a device are also of
   interest.  For AC power supply in particular, several Power
   Attributes beyond the real power are of potential interest to Energy
   Management Systems.  The set of these properties includes the complex
   Power Attributes (apparent power, reactive power, and phase angle of
   the current or power factor) as well as the actual voltage, the
   actual AC frequency, the Total Harmonic Distortion (THD) of voltage
   and current, and the impedance of an AC phase or of the DC supply.  A
   new standard for monitoring these Power Attributes should be in line
   with already-existing standards, such as [IEC.61850-7-4].

   For some network management tasks, it is desirable to receive
   notifications from entities when their power value exceeds or falls
   below given thresholds.

### Real Power / Power Factor

   The standard must provide means for reporting the real power for each
   Power Interface as well as for an entity.  Reporting power includes
   reporting the direction of power flow.

### Power Measurement Interval

   The standard must provide means for reporting the corresponding time
   or time interval for which a power value is reported.  The power
   value can be measured at the corresponding time or averaged over the
   corresponding time interval.

### Power Measurement Method

   The standard must provide means to indicate the method used to obtain
   these values.  Based on how the measurement was conducted, it is
   possible to associate a certain degree of confidence with the
   reported power value.  For example, there are methods of measurement
   such as direct power measurement, estimation based on performance
   values, or hard-coding average power values for an entity.

### Accuracy of Power and Energy Values

   The standard must provide means for reporting the accuracy of
   reported power and energy values.

### Actual Voltage and Current

   The standard must provide means for reporting the actual voltage and
   actual current for each Power Interface as well as for a device.  For
   AC power supply, means must be provided for reporting the actual
   voltage and actual current per phase.

### High-Power/Low-Power Notifications

   The standard must provide means for creating notifications if power
   values of an entity rise above or fall below given thresholds.

### Complex Power / Power Factor

   The standard must provide means for reporting the complex power for
   each Power Interface and for each phase at a Power Interface.  In
   addition to the real power, at least two of the following three
   quantities need to be reported: apparent power, reactive power, and
   phase angle.  The phase angle can be substituted by the power factor.

### Actual AC Frequency

   The standard must provide means for reporting the actual AC frequency
   for each Power Interface.

### Total Harmonic Distortion

   The standard must provide means for reporting the Total Harmonic
   Distortion (THD) of voltage and current for each Power Interface.
   For AC power supply, means must be provided for reporting the THD per
   phase.

### Power Supply Impedance

   The standard must provide means for reporting the impedance of a
   power supply for each Power Interface.  For AC power supply, means
   must be provided for reporting the impedance per phase.

## Power State

   Many entities have a limited number of discrete Power States.

   There is a need to report the actual Power State of an entity and to
   provide the means for retrieving the list of all supported Power
   States.

   Different standards bodies have already defined sets of Power States
   for some entities, and others are creating new Power State sets.  In
   this context, it is desirable that the standard support many of these
   Power State standards.  In order to support multiple management
   systems that possibly use different Power State sets while
   simultaneously interfacing with a particular entity, the Energy
   Management System must provide means for supporting multiple Power
   State sets used simultaneously at an entity.

   Power States have parameters that describe their properties.  It is
   required to have a standardized means for reporting some key
   properties, such as the typical power of an entity in a certain
   state.

   There is also a need to report statistics on Power States, including
   the time spent as well as the received and provided energy in a Power
   State.

### Actual Power State

   The standard must provide means for reporting the actual Power State
   of an entity.

### List of Supported Power States

   The standard must provide means for retrieving the list of all
   potential Power States of an entity.

### Multiple Power State Sets

   The standard must provide means for supporting multiple Power State
   sets simultaneously at an entity.

### List of Supported Power State Sets

   The standard must provide means for retrieving the list of all Power
   State sets supported by an entity.

### List of Supported Power States within a Set

   The standard must provide means for retrieving the list of all
   potential Power States of an entity for each supported Power State
   set.

### Typical Power Per Power State

   The standard must provide means for retrieving the typical power for
   each supported Power State.

### Power State Statistics

   The standard must provide means for monitoring statistics per Power
   State, including the total time spent in a Power State, the number of
   times each state was entered, and the last time each state was
   entered.  More Power State statistics are addressed by the
   requirements in Section 5.5.3.

### Power State Changes

   The standard must provide means for generating a notification when
   the actual Power State of an entity changes.

## Energy

   The monitoring of electrical energy received or provided by an entity
   is a core function of Energy Management. Since energy is an
   accumulated quantity, it is always reported for a certain interval of
   time.  This can be, for example, the time from the last restart of
   the entity to the reporting time, the time from another past event to
   the reporting time, the last given amount of time before the
   reporting time, or a certain interval specified by two timestamps in
   the past.

   It is useful for entities to record their received and provided
   energy per Power State and report these quantities.

  In addition, it is also useful for entities to record energy attributes
  such as maximum wake up time, maximum sleep time, service interruption
  time, transition time, maximum packet throughput, maximum bit throughput
  and report these quantities.

### Energy Measurement

   The standard must provide means for reporting measured values of
   energy and the direction of the energy flow received or provided by
   an entity.  The standard must also provide the means to report the
   energy passing through each Power Interface.

### Energy Efficiency Measurement

  The standard must provide means for measuring the trade-off between
  service-level object and energy consumption. [ETSI-ES-203-136],
  [ITUT-L.1310], [ATIS-0600015.03.2013] provide methodology and test
  procedure for measuring such energy efficiency related metrics, which
  is defined as the throughput forwarded by 1 watt. The traffic loads and
  the weighted multipliers need to be clearly established in advance.

  Note that, based on the specific optimization policy (throughput, heat,
  energy source, etc.), different derived metrics should be computed at
  the controller level.

### Power Gain Measurement

  The standard must provide means for measuring power gain, which can
  be calculated by actual power to be consumed by the entity divided by the maximum
  power of the entity. In addition, the minimum power gain can also be
  measured and reported.

### Time Intervals

   The standard must provide means for reporting the time interval for
   which an energy value is reported.

### Energy Per Power State

   The standard must provide means for reporting the received and
   provided energy for each individual Power State.  This extends the
   requirements on Power State statistics described in Section 5.4.7.

## Time Series of Measured Values

   For some network management tasks, obtaining time series of measured
   values from entities, such as power, energy, etc., is
   required.

   In general, time series measurements could be obtained in many
   different ways.  Means should be provided to either push such values
   from the location where they are available to the management system
   or to have them stored locally for a sufficiently long period of time
   such that a management system can retrieve the full time series.

   The following issues are to be considered when designing time series
   measurement and reporting functions:

   1.  Which quantities should be reported?

   2.  Which time interval type should be used (total, delta, sliding
       window)?

   3.  Which measurement method should be used (sampled, continuous)?

   4.  Which reporting model should be used (push or pull)?

   The most discussed and probably most needed quantity is energy.  But
   a need for others, such as power, can be identified as well.

   There are three time interval types under discussion for accumulated
   quantities such as energy.  They can be reported as total values,
   accumulated between the last restart of the measurement and a certain
   timestamp.  Alternatively, energy can be reported as delta values
   between two consecutive timestamps.  Another alternative is reporting
   values for sliding windows as specified in [IEC.61850-7-4].

   For non-accumulative quantities, such as power, different measurement
   methods are considered.  Such quantities can be reported using values
   sampled at certain timestamps or, alternatively, by mean values for
   these quantities averaged between two (consecutive) timestamps or
   over a sliding window.

   Finally, time series can be reported using different reporting
   models, particularly push-based or pull-based.  Push-based reporting
   can, for example, be realized by reporting power or energy values
   using the NETCONF protocol {{?RFC6241}}.  The NETCONF a protocol can
   also be used to realize pull-based reporting of time series.

   For reporting time series of measured values, the following
   requirements have been identified.  Further decisions concerning
   issues discussed above need to be made when developing concrete
   Energy Management standards.

### Time Series of Energy Values

   The standard must provide means for reporting time series of energy
   values.  If the comparison of time series between multiple entities
   is required, then time synchronization between those entities must be
   provided (for example, with the Network Time Protocol {{?RFC5905}}).

### Time Series Interval Types

   The standard must provide means for supporting alternative interval
   types.  The requirement in Section 5.5.2 applies to every reported
   time value.

### Time Series Storage Capacity

   The standard should provide means for reporting the number of values
   of a time series that can be stored for later reporting.

# Reporting on Lifecycle Management

   Lifecycle information related to manufacturing energy costs, transport,
   recyclability, and end-of-life disposal impacts is part of what is
   called "embedded carbon." This information is considered to be an
   estimated value, which might not be implemented today in the network
   devices. It might be part of the vendor information, and to be collected
   from datasheets or databases. In accordance with ISO 14040/44, this
   information should be considered as part of the sustainable strategy
   related to energy efficiency. Also, refer to the ecodesign framework
   [(EU) 2024/1781] published in June by the European Commission.

## Carbon Reporting

   To report on carbon equivalents for global reporting, it is important
   to correlate the location where the specific entity/network element
   is operating with the corresponding carbon factor. Refer to the world
   emission factor from the International Energy Agency (IEA), electricity
   maps applications that reflect the carbon intensity of the electricity
   consumed, etc.

## Energy Mix

   To facilitate carbon reporting for global reporting, it is important to
   know the type of energy that network devices are consuming, such as
   solar energy, wind energy, cogeneration, etc.

# Control of Entities

   Many entities control their Power State locally.  Other entities need
   interfaces for an Energy Management System to control their Power
   State.

   A power supply is typically not self-managed by devices, and control
   of a power supply is typically not conducted as an interaction
   between an Energy Management System and the device itself.  It is
   rather an interaction between the management system and a device
   providing power at its Power Outlets.  Similar to Power State
   control, power supply control may be policy driven.  Note that
   shutting down the power supply abruptly may have severe consequences
   for the device.

## Provisioning Power States

   The standard must provide means for provisioning Power States of entities.

   When an Energy Object is set to a particular Power State, the
   represented device or component may be busy.  The Energy Object
   should set the desired Power State and then update the actual Power
   State when the device or component changes. The standard must
   provide means to report the intented and applied Power States,
   with the Network Management Datastore Architecture (NMDA) {{?RFC8342}}

## Controlling Power SupplyProvisioning

   The standard must provide means for switching a power supply off or
   turning a power supply on at Power Interfaces providing power to one
   or more devices.

## Controlling Energy Saving and Optimization Functionalities

  The standard must provide means for controlling energy saving and
  optimization functionalities and allocating the committed component resource
  (e.g., adjust fan speed, shutdown high speed interface) or committed device
  resource (e.g., multiple cards scheduling, multiple power module scheduling).

  In addition, the standard must provide means to support both local management and
  network wide management based on energy saving functionality.

# Reporting an controlling on Other Entities

As discussed in Section 5, not all energy-related information may be
   available at the entity in question.  Such information may be
   provided by other entities.  This section groups the requirements for the discovery, the reporting and the control
   of information. 

The intend is to add a table in section 9.

## Discovery of Power inlet Entities

Energy consumption must not be accounted twice

### Reporting on Other Entities

   As discussed in Section 5, not all energy-related information may be
   available at the entity in question.  Such information may be
   provided by other entities.  This section covers only the reporting
   of information.  See Section 8 for requirements on controlling other
   entities.

   There are cases where a power supply unit switches power for several
   entities by turning power on or off at a single Power Outlet or where
   a power meter measures the accumulated power of several entities at a
   single power line.  Consequently, it should be possible to report
   that a monitored value does not relate to just a single entity but is
   an accumulated value for a set of entities.  All of the entities
   belonging to that set need to be identified.

### Reports on Other Entities

   The standard must provide means for an entity to report information
   on another entity.

### Identity of Other Entities on Which Information Is Reported

   For entities that report on one or more other entities, the standard
   must provide means for reporting the identity of other entities on
   which information is reported.  Note that, in some situations, a
   manual configuration might be required to populate this information.

### Reporting Quantities Accumulated over Multiple Entities

   The standard must provide means for reporting the list of all
   entities from which contributions are included in an accumulated
   value.

### List of All Entities on Which Information Is Reported

   For entities that report on one or more other entities, the standard
   must provide means for reporting the complete list of all those
   entities on which energy-related information can be reported.

### Content of Reports on Other Entities

   For entities that report on one or more other entities, the standard
   must provide means for indicating what type or types of energy-
   related information can be reported, and for which entities.

## Controlling Other Entities

   This section specifies requirements for controlling Power States and
   power supply of entities by communicating with other entities that
   have the means for doing that control.

### Controlling Power States of Other Entities

   RFC6988 allow some entities have control over Power States of other entities,
   e.g., in Building automation case where a gateway to a building system may have
   the means to control the Power State of entities in the building that do not have
   an IP interface.

   In this document, we assume all network devices have IP connectivity in the operator
   controlled environment. Therefore only an Energy Management System has control over
   Power States of other entities.

   In addition, it is required that an entity that has its state
   controlled by the Energy Management System has the means to report the list of
   these other entities.

#### Control of Power States of Other Entities

   The standard must provide means for an Energy Management System to
   send Power State control commands to an entity that controls the
   Power States of entities other than the entity to which the command
   was sent.

#### Identity of Other Power State Controlled Entities

   The standard must provide means for reporting the identities of the
   entities for which the reporting entity has the means to control
   their Power States.  Note that, in some situations, a manual
   configuration might be required to populate this information.

#### List of All Power State Controlled Entities

   The standard must provide means for an entity to report the list of
   all entities for which it can control the Power State.

#### List of All Power State Controllers

   The standard must provide means for an entity that receives commands
   controlling its Power State from other entities to report the list of
   all those entities.

### Controlling Power Supply

   Some entities may have control of the power supply of other entities,
   for example, because the other entity is supplied via a Power Outlet
   of the entity.  For this and similar cases, means are needed to make
   this control accessible to the Energy Management System.  This need
   is already addressed by the requirement in Section 6.2.

   In addition, it is required that an entity that has its supply
   controlled by other entities has the means to report the list of
   these other entities.  This need is already addressed by requirements
   in Sections 5.2.3 and 5.2.4.


# Requirements extracted from proponents drafts

This section extracts and groups requirements from the documents of the proponents of the GREEN WG creation [GREEN-BOF], [sustainability-insights], [legacy-path] and [rfc6988bis-green]. The aim is to determine initial sets of requirements actionable at different levels of the framework presented below {{framework}}.

## Operator'requirements from [charter-refinement] document

The table below is a copy of the operator'requirements table of [charter-refinement]. They are based on the inputs received from operators for the GREEN BoF [operators-inputs].

|category|requirements|note|Priority|
|:----|:----|:----|:----|
|Observability|Component granularity, e.g., per line-card, per-port|Per component measurement|1|
|Observability|Availability of information on the power consumption of the device, without needing instrumentation connected to the infrastructure|Related to connected device case|1|
|Observability|Triggering of alarms when consumption deviate from a nominal usage|Alarm notification|1??|
|Observability|Improvement of metering solutions (finer granularity, control of the energy efficiency and saving, interoperability, exposure)|Standardized metering??|1|
|Analysis|Common definition of energy efficiency in network devices/components|Standard metric|1|
|Analysis|Common methodology of measurements for fair comparison|Standard methodology|2|
|Analysis|How to provide accurate figures (context of the measurement in terms of time period, location, traffic, etc|Time based, location based visualization|2 ??|
|Analysis|Database for decision in case of large data transfer|Information Correlation|3|
|Analysis|Ability of multi-layer analysis (e.g., IP plus optical)|POI Use Case|3|
|Control& Mgmt|To have devices with elastic power consumption according to the carried traffic|Dynamic Energy Saving|2|
|Control& Mgmt|Support of network-wide energy saving and optimization functions|Network Level Mgmt|2|
|Control& Mgmt|Support of network-wide control of energy optimization APIs, allowing external applications to optimize consumption|Network Level Mgmt|2|
|Control& Mgmt|Advanced sleep mode, needing some sort of low power mode when node is lightly utilized|Dynamic Energy Saving|2|
|Control& Mgmt|Ability to steer traffic based on power savings|Traffic Engineering|4|
|Control& Mgmt|Comparison of decision vs optimal case|Intent based Concept|2|
|Control& Mgmt|Synchronous query support|Network Level Query|2|
|Inventory Management|Inventory of power components (of devices, racks, etc) including together|Component & Device Level|1|
|Interaction with other domain|Inclusion of data center networks in the picture|Data Center Case|3|
|Interaction with other domain|Inclusion of data center networks in the picture|Mobile Network Case|3|
|Sustainability & Carbon Emission|Optimize the overall CO2 footprint (i.e., energy mix based on source type) facilitating the engineering of PoP More renewable energy|More renewable energy|4|
|Sustainability & Carbon Emission|Support GHG units|Measurement Units|4|
|Sustainability & Carbon Emission|Support Energy units|More renewable energy|2 ??|
|Sustainability & Carbon EmissiCarbon, renewable|4|
|Sustainability & Carbon Emission|Accounting of legacy installed based GHG/energy|Accounting Cost|4|
|Sustainability & Carbon Emission|Track device/network Energy Consumption Before Operation|Manufacturing, transport(weight, volume, package)|4|

## Requirements extracted from [legacy-path]

|category|requirements|note|Priority|
|:----|:----|:----|:----|
|Inventory Management|component control capacity (aka component max on/off frequency supported)|Per component control| 1 (i)|
|Analysis|assess the gains of introducing eco-designed components in a device|Device Level Mgmt| 1 (ii)|
|Control& Mgmt|comprehensive support of network-wide energy efficiency includes legacy devices|Network Level Mgmt| 1 (iii)|

(i) Avoid control to break the component

(ii) the gain must be measurable

(iii) network-wide solution must include legacy devices and green-wg ready devices

## Requirements from [rfc6988bis-green] draft Open Issues

|category|requirements|note|Priority|
|:----|:----|:----|:----|
|Control& Mgmt|Distinguish backup sources|rfc6988bis battery| 2|
|Inventory Management|Reporting on Other Entities, typically smart PDU or PoE|Fit in "Inventory of power components (of devices, racks, etc) including together"| 2|
|Observability or Interaction with Other domain|Room sensor (hvac...)|Data Center Case| 4|
|Observability|flexible (future-proof) description of the nature of the sources of the energy used |Standard metric|2 |

## Requirements extracted from [sustainability-insights] uses cases

There are limited to energy consumption vs sustainability

|category|requirements|note|Priority|
|:----|:----|:----|:----|
|Observability|Provide near-real-time energy consumption to different device types, service types, and individual users | Helps identify which devices or network functions are consuming more energy. | 2 |
|Migration or Upgrade | Provide KPIs for energy efficiency parameters, enhance accuracy of upgrade decisions | Helps make informed decisions about upgrades based on actual usage data. |   |
|Recycling | Report on percentage of recycled user devices and components. Enable comprehensive reporting and recycling efforts | Major driver of the circular economy, transparency is key | 4 |
|Power Optimization | Provide KPIs for energy efficiency parameters. Perform actions to reduce energy consumption | Monitor network and application performance to optimize power usage | 4 |
|Control& Mgmt Switch off | Stop and restart WiFi APs with the right time, space, and service granularity | Save power consumption during periods when APs are not in use.| 2|

## Framework Discussed During the BoF {#framework}

The overall framework is shown in {{green-framework}}.

~~~~

       What needs to be standardized for Framework


(3) Network Domain Level :

(a)              (b)              (c)
Inventory        Monitor       +- DataSheets/DataBase and/or via API
Of identity      Energy        |  Metadata and other device/component
and Capability   Efficiency    |  /network related information:
     ^               ^         |
     |               |         |  .Power/Energy related metrics
     |               |         |  .information
     |               |         |  .origin of Energy Mix
     |               |         |  .carbon aware based on location
     |               |         |
     |               |         |
     |               |         |
     |               |         v
+--------------------------------------------------------------------+
|                   *                                                |
|     (2) controller   (collection, compute and aggregate?)          |
|                                                                    |
+--------------------------------------------------------------------+
             ^              ^                   ^ |
  (d)        |  (e)         |  (f)              | |(g)
  Inventory  |  Monitor     |  GREEN WG:        | |GREEN WG: Control
  Capability |  Traffic     |  Monitor power    | |(Energy saving
             |  & power     |  Proportion,      | |Functionality
             |  consumption |  Energy efficiency| |Localized mgmt/
             |              |  ratio, etc)      | |network wide mgmt)
             |              |                   | |
             |              |                   | |
             |              |                   | v
+--------------------------------------------------------------------+
|                                            *                       |
|                  (1) Device/Component Level                        |
|                                                                    |
| +---------+  +-----------+  +----------------+  +----------------+ |
| | (I)     |  | (II)      |  | (III)          |  | (IV)           | |
| | Network |  | Device    |  | Legacy Network |  | 'Attached'(PoE | |
| | Device  |  | Component |  | Device         |  | kind) Device   | |
| |         |  |           |  |                |  |                | |
| +---------+  +-----------+  +----------------+  +----------------+ |
+--------------------------------------------------------------------+

(*) Energy Efficiency Management Function is implemented inside the
device or in a controller

~~~~
{: #green-framework title="Framework discussed during the BoF"}

The main elements in the framework are as follows:

(a),(d) Inventory

(b),(c) GREEN Metrics

(b),(f) Monitor energy efficiency

(e) Monitor power consumption and traffic (IPPM WG throughput, traffic load, etc)

(g) Control Energy Saving

# Use Cases

The following sub-sections present a number of use cases from where to elicit the relevant requirements to be supported for Energy Efficiency Management.

## Selective reduction of energy consumption in network parts proportional to traffic levels

Traffic levels in a network follow patterns reflecting the behavior of consumers. Those patterns show periodicity in the terms of the traffic delivered, that can range from daily (from 00:00 to 23:59) to seasonal (e.g., winter to summer), showing peaks and valleys that could be exploited to reduce the consumption of energy in the network proportionally, in case the underlying network elements incorporate such capabilities. The reduction of energy consumption could be performed by leveraging on sleep modes in components up to more extreme actions such as switching off network components or modules. Such decisions are expected to no impact on the service delivered to customers, and could be accompanied by traffic relocation and / or concentration in the network. 
 
For this use case, the following requirements apply:

|category|requirements|note|Priority|
|:----|:----|:----|:----|
|Observability|Component granularity, e.g., per line-card, per-port|Per component measurement|1|
|Observability|Availability of information on the power consumption of the device, without needing instrumentation connected to the infrastructure|Related to connected device case|1|
|Analysis|Common definition of energy efficiency in network devices/components|Standard metric|1|
|Analysis|Common methodology of measurements for fair comparison|Standard methodology|2|
|Analysis|Ability of multi-layer analysis (e.g., IP plus optical)|POI Use Case|3|
|Control& Mgmt|To have devices with elastic power consumption according to the carried traffic|Dynamic Energy Saving|2|
|Control& Mgmt|Support of network-wide energy saving and optimization functions|Network Level Mgmt|2|
|Control& Mgmt|Advanced sleep mode, needing some sort of low power mode when node is lightly utilized|Dynamic Energy Saving|2|
|Control& Mgmt|Ability to steer traffic based on power savings|Traffic Engineering|4|

(Note: revision of the identified requirements is needed; this is just an intial exercise).

## (Additional use casess)

To be done. Additional use cases will be described in future versions of this document.


# Security Considerations

   Controlling Power State and power supply of entities are considered
   highly sensitive actions, since they can significantly affect the
   operation of directly and indirectly connected devices.  Therefore,
   all control actions addressed in Sections 6 and 8 must be
   sufficiently protected through authentication, authorization, and
   integrity protection mechanisms.

   Entities that are not sufficiently secure to operate directly on the
   public Internet do exist and can be a significant cause of risk, for
   example, if the remote control functions described in Sections 6 and
   8 can be exercised on those devices from anywhere on the Internet.
   The standard needs to provide means for dealing with such cases.  One
   solution is providing means that allow the isolation of such devices,
   e.g., behind a sufficiently secured gateway.  Another solution is to
   allow compliant implementations to disable sensitive functions, or to
   not implement such functions at all.

   The monitoring of energy-related quantities of an entity as addressed
   in Sections 5 through 8 can be used to derive more information than
   just the received and provided energy; therefore, monitored data
   requires protection.  This protection includes authentication and
   authorization of entities requesting access to monitored data as well
   as confidentiality protection during transmission of monitored data.
   Privacy of stored data in an entity must be taken into account.
   Monitored data may be used as input to control, accounting, and other
   actions, so integrity of transmitted information and authentication
   of the origin may be needed.

## Secure Energy Management

   The standard must provide privacy, integrity, and authentication
   mechanisms for all actions addressed in Sections 5 through 8.  The
   security mechanisms must meet the security requirements detailed in
   Section 1.4 of {{?RFC3411}}.

## Isolation of Insufficiently Secure Entities

   The standard must provide means to allow the isolation of entities
   that are not sufficiently secure to operate on the public Internet,
   e.g., behind a gateway that implements sufficient security that the
   vulnerable entities are not directly exposed to the Internet.

## Optional Restriction of Functions

   The standard must allow compliant implementations to disable
   sensitive functions, or to not implement such functions at all, when
   operating in environments that are not sufficiently secured.  This
   applies particularly to the control functions described in Sections 6
   and 8.

# Acknowledgments

   The authors would like to thank ...

# Open Issues

   o Do we need to keep a reference to the MIB object entPhysicalUUID (in section 4.4 from ENTITY-MIB v4) in case of legacy device (MIB)?

   o The EMAN requirements and EMAN framework had a lot of emphasis on the "Reporting on Other Entities", typically smart PDU or PoE.
   Is this important? Should this be removed? Should it be addressed in a future charter?
   This is text about "Sections 7 and 8 contain requirements specific to Energy Management. Due to the nature of power supply, some monitoring and control functions are not conducted by interacting with the entity of interest but rather with other entities, for example, entities upstream in a power distribution tree."

   Expressed differently: Out of scope for the short term approach of EMAN framework enhancements, but might be good to call it out, EMAN
   doesn't include mechanisms for integrating occupancy sensors or user behavior analytics, which can be critical for optimizing HVAC, lighting, and other systems for energy efficiency. This is a key aspect for Smart Buildings and Data Centers energy efficiency metrics.

   o It's not clear whether we need new Power State (Set)? Maybe not but we need to explain the mapping of existing energy efficient features to specific Power States.

   o basic (scalar) units are not enough to describe Power Data Unit capabilities and/or output. We need a more complex structure (which might already exist?)
   to cover and combine meanings (that I copied from the chats) like CO2 footprint, clean energy, mix, renewable. as an example, this should help to describe
   reduction of energy consumption and the increase of renewable energy consumption

   o Enhance EMAN framework, to support a more robust and comprehensive
   Energy Efficiency Strategy. Let devices report whatever they can using
   existing interfaces, without waiting until they implement new capabilities
   determined by new or existing standards. Including the capability to
   integrate with external data sources (for example, for devices that don't
   have the capability or reporting any energy-related metrics) such as vendor
   datasheets that provide energy consumption. Use case => upgrading a device
   for better Energy Efficiency Management. Not sure whether framework-related
   requirements should be covered here.

   o Leveraging existing devices modularity to introduce eco-designed components
   in the networks while being able to assess the gains in sustainability.
   https://datatracker.ietf.org/doc/html/draft-stephan-legacy-path-eco-design-01
   https://github.com/emile22/sustainability

   o Discuss the need to reflect component on/off frequency capacity (in YANG)
   to avoid too intensive power on/off.

   o Discuss the need to support a description of the different nature of the
   sources of the energy used (mix). It should be flexible are the types of sources
   might augment in the future.

   o Company's SBTi approved decarbonization plan and how to link it to
   GREEN WG scope, short/mid vs long term.

   The Science Based Targets initiative(SBTi)[https://sciencebasedtargets.org]
   defines and promotes best practice in science-based target setting. Offering
   a range of target-setting resources and guidance, the SBTi independently
   assesses and approves companies’ targets in line with its strict criteria.

   Open issue, https://github.com/marisolpalmero/GREEN-bof/issues/88

   o Consideration to include in scope, allocate/compute and report the energy
   spent on behalf of a particular customer/user.
   Open issue, marisolpalmero/GREEN-bof#89

# References

## Normative References

   [IEC.61850-7-4]
              International Electrotechnical Commission, "Communication
              networks and systems for power utility automation --
              Part 7-4: Basic communication structure -- Compatible
              logical node classes and data object classes", March 2010.

   [IEC.62053-21]
              International Electrotechnical Commission, "Electricity
              metering equipment (a.c.) -- Particular requirements --
              Part 21: Static meters for active energy (classes 1
              and 2)", January 2003.

   [IEC.62053-22]
              International Electrotechnical Commission, "Electricity
              metering equipment (a.c.) -- Particular requirements --
              Part 22: Static meters for active energy (classes 0,2 S
              and 0,5 S)", January 2003.

   [IEEE-100] IEEE, "The Authoritative Dictionary of IEEE Standards
              Terms, IEEE 100, Seventh Edition", December 2000.

   [IEEE-1621]
              Institute of Electrical and Electronics Engineers,
              "IEEE 1621-2004 - IEEE Standard for User Interface
              Elements in Power Control of Electronic Devices Employed
              in Office/Consumer Environments", 2004.


   [ATIS-0600015.03.2013]
              ATIS, "ATIS-0600015.03.2013: Energy Efficiency for
              Telecommunication Equipment: Methodology for Measurement
              and Reporting for Router and Ethernet Switch Products",
              2013.

   [ETSI-ES-203-136]
              ETSI, "ETSI ES 203 136: Environmental Engineering (EE);
              Measurement methods for energy efficiency of router and
              switch equipment", 2017, <https://www.etsi.org/deliver/
              etsi_es/203100_203199/203136/01.02.00_50/
              es_203136v010200m.pdf>.

   [ITUT-L.1310]
              ITU-T, "L.1310 : Energy efficiency metrics and measurement
              methods for telecommunication equipment", 2020,
              <https://www.itu.int/rec/T-REC-L.1310/en>.

# Appendix

This appendix should be removed in the next version

## In Preparation of the GREEN BoF at IETF 120

   The EMAN (Energy MANagement) working group (WG), created in 2010 and now concluded, has produced multiples RFCs

      * {{?RFC7603}}, Energy Management (EMAN) Applicability Statement

      * {{?RFC7577}}, Definition of Managed Objects for Battery Monitoring

      * {{?RFC7460}}, Monitoring and Control MIB for Power and Energy

      * {{?RFC7461}}, Energy Object Context MIB

      * {{?RFC7326}}, Energy Management Framework

      * {{?RFC6988}}, Requirements for Energy Management

      * {{?RFC6933}}, Entity MIB (Version 4)

   Note also that some other energy-related MIB modules have been created, but not by the EMAN Working Group

      * {{?RFC3433}}, Entity Sensor MIB module

      * {{?RFC3621}}, Power Ethernet MIB modules

      * {{?RFC1628}}, UPS Power Monitoring MIB module

      * LLDP MIB module and LLDP MED MIB module

   Due to limitations regarding Writeable MIB module, one IESG statement published in 2014 encourages the use the NETCONF/YANG standards for configuration. Based on the YANG modules    developments, three MIB  modules (Entity MIB module, Entity Sensor MIB module, Entity State MIB module) have been converted into the "YANG Data Model for Hardware Management" {{?RFC8348}}.

   However, Power and Energy Monitoring and Control MIB modules has not been converted yet into YANG modules.

   Eleven years after the EMAN requirements RFC 6988 publication, this document re-evaluates the energy-related requirements, as a preparation for the GREEN BoF at IETF 120.

## High-level Differences with RFC6988

The following section will delve into the specific details but from a high level point of view, the differences between this document and the RFC6988 are:

- New definition for "Energy Efficiency Management"

- A focus towards YANG, and not any longer on MIB modules

- As a consequence from the previous point, the ENTITY-MIB v4 {{?RFC6933}} is replaced by the Hardware YANG module RFC8348

- battery management is removed (as batteries haves some self-optimization features these days)

- Less focus on the Power over Ethernet management

- A focus on reporting lifecycle management, considering energy and transformation towards carbon awareness
