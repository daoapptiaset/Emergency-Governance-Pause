# Emergency-Governance-Pause
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract EmergencyGovernance {
    address public governance;
    bool public paused;
    uint256 public pauseDuration;
    uint256 public pauseEndTime;

    event EmergencyPaused(uint256 until);

    constructor(address _governance) {
        governance = _governance;
    }

    modifier whenNotPaused() {
        require(!paused || block.timestamp > pauseEndTime, "Contract paused");
        _;
    }

    function emergencyPause(uint256 _duration) public {
        require(msg.sender == governance, "Only governance");
        paused = true;
        pauseDuration = _duration;
        pauseEndTime = block.timestamp + _duration;
        emit EmergencyPaused(pauseEndTime);
    }

    function resume() public {
        require(msg.sender == governance, "Only governance");
        paused = false;
    }
}
