# Credits and Acknowledgments

## 🙏 Special Thanks

This UniFi LED Scheduler project would not have been possible without the incredible work of the **Art of WiFi** team and the broader UniFi community.

## Art of WiFi - UniFi API Client

The core functionality of this project is built upon the [Art-of-WiFi/UniFi-API-client](https://github.com/Art-of-WiFi/UniFi-API-client) PHP library.

### Why This Library is Amazing

The Art of WiFi team has:
- ✅ Reverse-engineered the UniFi Controller API
- ✅ Created a clean, well-documented PHP interface
- ✅ Maintained the library for years with consistent updates
- ✅ Provided extensive examples and documentation
- ✅ Built a comprehensive API reference covering hundreds of endpoints
- ✅ Made it available to the community under an open license

### Key Contributors

**Special recognition to:**
- The [Art of WiFi](https://github.com/Art-of-WiFi) organization
- All the contributors who have submitted issues, pull requests, and improvements
- The community members who have tested and documented various UniFi Controller versions

Without their dedication to reverse-engineering and documenting Ubiquiti's private API, projects like this scheduler simply wouldn't exist.

## Resources

### UniFi API Client Documentation
- **Main Repository**: https://github.com/Art-of-WiFi/UniFi-API-client
- **API Reference**: https://github.com/Art-of-WiFi/UniFi-API-client/blob/master/API_REFERENCE.md
- **Example Scripts**: https://github.com/Art-of-WiFi/UniFi-API-client/tree/master/examples
- **Composer Package**: https://packagist.org/packages/art-of-wifi/unifi-api-client

### Key Features We Use
- `login()` - Authentication with UniFi Controller
- `list_devices()` - Retrieving access point information
- `led_override()` - Controlling AP LED states
- Multi-site support via site_id parameter
- SSL/TLS handling for secure connections

## Technology Stack

Beyond the UniFi API client, this project also relies on:

### Core Technologies
- **PHP 8.2** - Modern PHP runtime
- **Docker** - Containerization platform
- **Alpine Linux** - Lightweight container base image
- **Supercronic** - Container-friendly cron daemon
- **Composer** - PHP dependency management

### PHP Libraries (via Composer)
- `art-of-wifi/unifi-api-client` - UniFi Controller API client
- `vlucas/phpdotenv` - Environment variable management
- `symfony/polyfill-*` - PHP compatibility layers

## Open Source Philosophy

This project stands on the shoulders of giants. The open-source community's willingness to:
- Share knowledge freely
- Document undocumented APIs
- Build tools for the common good
- Support each other through issues and discussions

...is what makes projects like this possible.

## Contributing Back

If this project has been helpful to you, consider:

1. **⭐ Star the repositories**:
   - [Art-of-WiFi/UniFi-API-client](https://github.com/Art-of-WiFi/UniFi-API-client)
   - This repository

2. **📖 Share your use case**: Help others by documenting how you're using the scheduler

3. **🐛 Report issues**: Help improve both projects by reporting bugs

4. **💡 Contribute**: Submit pull requests with improvements or new features

5. **☕ Support the maintainers**: If the Art of WiFi project has a sponsorship option, consider supporting them

## Related Projects

Other great projects using the UniFi API client:

- Check the [Art-of-WiFi organization](https://github.com/Art-of-WiFi) for other UniFi-related tools
- Browse the [dependent repositories](https://github.com/Art-of-WiFi/UniFi-API-client/network/dependents) to see what others are building

## License Information

- **This Project**: See LICENSE file in this repository
- **UniFi API Client**: See [Art-of-WiFi/UniFi-API-client LICENSE](https://github.com/Art-of-WiFi/UniFi-API-client/blob/master/LICENSE)

## Contact & Support

For issues related to:
- **This LED Scheduler**: Open an issue in this repository
- **UniFi API Client**: Visit https://github.com/Art-of-WiFi/UniFi-API-client/issues
- **UniFi Controller**: Contact Ubiquiti support

---

**Thank you to everyone who contributes to open source networking tools!** 🎉

Your work makes it possible for hobbyists, professionals, and businesses to build amazing automation solutions.

---

*Last Updated: January 2026*

