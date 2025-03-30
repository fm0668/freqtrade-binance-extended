# Freqtrade - Binance Extended Version

This is a modified version of Freqtrade with extended Binance K-line data support.

## Version Information

**Current version:** 2025.4.0a1 (Alpha release with Binance extended K-line data support)

## Modifications

This version includes the following enhancements:
- Extended Binance K-line data support with additional fields:
  - Quote asset volume
  - Number of trades
  - Taker buy base asset volume
  - Taker buy quote asset volume
  - Close time
- Additional data provider methods to access extended fields in strategies
- Customized data handling for Binance exchange